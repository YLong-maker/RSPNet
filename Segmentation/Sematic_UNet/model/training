import math
from functools import partial

import torch
import torch.nn as nn
import torch.nn.functional as F


def CE_Loss(inputs, target, cls_weights, num_classes=21):
    n, c, h, w = inputs.size()  # 输入大小，n=batch_size，c=类别数，h=高度，w=宽度
    nt, ht, wt = target.size()  # 目标大小，nt=batch_size，ht=目标图像的高度，wt=目标图像的宽度

    # 如果输入和目标的大小不一致，则进行插值操作
    if h != ht and w != wt:
        inputs = F.interpolate(inputs, size=(ht, wt), mode="bilinear", align_corners=True)

    # 将输入和目标调整为合适的形状
    temp_inputs = inputs.transpose(1, 2).transpose(2, 3).contiguous().view(-1, c)  # 调整为 [n*h*w, c]
    # temp_inputs = inputs.view(-1, c)  # 变为 [n*h*w, c]
    temp_target = target.view(-1)  # 展平目标标签为 [n*h*w]

    # 计算交叉熵损失
    CE_loss = nn.CrossEntropyLoss(weight=cls_weights, ignore_index=num_classes)(temp_inputs, temp_target)
    return CE_loss


"""
这段代码实现了 Focal Loss，这是一个改进版的交叉熵损失，
专门用于处理类别不平衡问题。它通过对 困难样本（即模型不确定的样本）
施加更高的权重来增强模型对这些样本的关注，尤其是在训练过程中遇到类不平衡时。
"""
def Focal_Loss(inputs, target, cls_weights, num_classes=21, alpha=0.5, gamma=2):
    n, c, h, w = inputs.size()  # 输入的尺寸，n=batch_size，c=类别数，h=高度，w=宽度
    nt, ht, wt = target.size()  # 目标标签的尺寸，nt=batch_size，ht=目标高度，wt=目标宽度

    # 如果输入和目标的尺寸不一致，则进行插值调整
    if h != ht and w != wt:
        inputs = F.interpolate(inputs, size=(ht, wt), mode="bilinear", align_corners=True)

    # 展平输入和目标，使其适应交叉熵损失函数的要求
    temp_inputs = inputs.transpose(1, 2).transpose(2, 3).contiguous().view(-1, c)
    # temp_inputs = inputs.view(-1, c)  # 输入变为 [n*h*w, c]
    temp_target = target.view(-1)  # 目标变为 [n*h*w]

    # 计算交叉熵损失，使用 `reduction='none'` 返回每个像素的损失
    logpt = -nn.CrossEntropyLoss(weight=cls_weights, ignore_index=num_classes, reduction='none')(temp_inputs,
                                                                                                 temp_target)
    pt = torch.exp(logpt)  # 计算每个像素属于真实类别的概率

    # 如果 alpha 存在，则乘上 alpha 来调节正负样本的权重
    if alpha is not None:
        logpt *= alpha

    # 计算 Focal Loss
    loss = -((1 - pt) ** gamma) * logpt

    # 对所有像素的损失求平均
    loss = loss.mean()
    return loss


"""
这段代码实现了 Dice Loss，它是用于计算图像分割任务中的性能度量指标，
尤其适用于数据类别不平衡的场景。Dice 系数衡量的是预测结果和真实标签之间的重叠程度，
通常用于语义分割任务中，特别是在医学图像分割等领域。
"""
def Dice_loss(inputs, target, beta=1, smooth=1e-5):
    n, c, h, w = inputs.size()  # 输入的尺寸：n=batch_size, c=类别数, h=高度, w=宽度
    nt, ht, wt, ct = target.size()  # 目标标签的尺寸：nt=batch_size, ht=高度, wt=宽度, ct=类别数

    # 如果输入和目标的尺寸不一致，进行插值
    if h != ht and w != wt:
        inputs = F.interpolate(inputs, size=(ht, wt), mode="bilinear", align_corners=True)

    # 对输入和目标进行转换，保证计算时的一致性
    temp_inputs = torch.softmax(inputs.transpose(1, 2).transpose(2, 3).contiguous().view(n, -1, c), -1)
    # temp_inputs = torch.softmax(inputs.permute(0, 2, 3, 1).contiguous().view(n, -1, c), -1)
    temp_target = target.view(n, -1, ct)

    # 计算真正例 (tp)，即真实类别和预测类别的交集
    tp = torch.sum(temp_target[..., :-1] * temp_inputs, axis=[0, 1])
    # 计算假正例 (fp)，即预测为类别而真实标签为背景
    fp = torch.sum(temp_inputs, axis=[0, 1]) - tp
    # 计算假负例 (fn)，即真实标签为类别而预测为背景
    fn = torch.sum(temp_target[..., :-1], axis=[0, 1]) - tp

    # 计算 Dice 系数，beta 控制对假负样本的惩罚程度
    score = ((1 + beta ** 2) * tp + smooth) / ((1 + beta ** 2) * tp + beta ** 2 * fn + fp + smooth)
    # 计算 Dice Loss
    dice_loss = 1 - torch.mean(score)
    return dice_loss


def weights_init(net, init_type='normal', init_gain=0.02):
    def init_func(m):
        classname = m.__class__.__name__  # 获取模块的类名
        if hasattr(m, 'weight') and classname.find('Conv') != -1:  # 如果是卷积层
            if init_type == 'normal':
                torch.nn.init.normal_(m.weight.data, 0.0, init_gain)  # 正态分布初始化
            elif init_type == 'xavier':
                torch.nn.init.xavier_normal_(m.weight.data, gain=init_gain)  # Xavier初始化
            elif init_type == 'kaiming':
                torch.nn.init.kaiming_normal_(m.weight.data, a=0, mode='fan_in')  # Kaiming初始化
            elif init_type == 'orthogonal':
                torch.nn.init.orthogonal_(m.weight.data, gain=init_gain)  # 正交初始化
            else:
                raise NotImplementedError('initialization method [%s] is not implemented' % init_type)
        elif classname.find('BatchNorm2d') != -1:  # 如果是批量归一化层
            torch.nn.init.normal_(m.weight.data, 1.0, 0.02)  # 正态分布初始化，均值1，标准差0.02
            torch.nn.init.constant_(m.bias.data, 0.0)  # 偏置初始化为0

    print('initialize network with %s type' % init_type)  # 打印初始化方法
    net.apply(init_func)  # 将初始化函数应用到网络的每一层


def get_lr_scheduler(lr_decay_type, lr, min_lr, total_iters, warmup_iters_ratio=0.05, warmup_lr_ratio=0.1,
                     no_aug_iter_ratio=0.05, step_num=10):
    # 根据学习率衰减类型（lr_decay_type）选择不同的学习率调度方式
    def yolox_warm_cos_lr(lr, min_lr, total_iters, warmup_total_iters, warmup_lr_start, no_aug_iter, iters):
        """
                YOLOX特定的余弦衰减带热身策略的学习率调度函数。
                根据当前的训练迭代次数 (iters) 来动态调整学习率。

                参数：
                lr: 当前的学习率
                min_lr: 最小学习率
                total_iters: 总的训练迭代次数
                warmup_total_iters: 热身阶段的迭代次数
                warmup_lr_start: 热身阶段开始时的学习率
                no_aug_iter: 不进行数据增强的迭代次数
                iters: 当前的训练迭代次数

                返回：
                调整后的学习率
        """
        if iters <= warmup_total_iters:
            # 在热身阶段，使用一个二次函数来逐步增加学习率
            lr = (lr - warmup_lr_start) * pow(iters / float(warmup_total_iters), 2) + warmup_lr_start
        elif iters >= total_iters - no_aug_iter:
            # 如果迭代数接近总迭代数，且接近不使用数据增强的迭代次数，则将学习率设置为最小学习率
            lr = min_lr
        else:
            # 在其他阶段，使用余弦衰减函数来调整学习率
            lr = min_lr + 0.5 * (lr - min_lr) * (
                    1.0 + math.cos(
                math.pi * (iters - warmup_total_iters) / (total_iters - warmup_total_iters - no_aug_iter))
            )
        return lr

    def step_lr(lr, decay_rate, step_size, iters):
        """
                逐步衰减的学习率调度函数。

                参数：
                lr: 当前的学习率
                decay_rate: 衰减率，用来控制每个step后学习率的减少比例
                step_size: 每次衰减发生的步数
                iters: 当前的训练迭代次数

                返回：
                调整后的学习率
        """
        if step_size < 1:
            raise ValueError("step_size must above 1.")
        # 计算经过了多少个衰减步
        n = iters // step_size
        # 按照衰减率调整学习率
        out_lr = lr * decay_rate ** n
        return out_lr

    # 根据学习率衰减类型选择合适的调度函数
    if lr_decay_type == "cos":
        # 如果选择的是余弦衰减（cos），计算热身阶段的迭代次数和学习率
        warmup_total_iters = min(max(warmup_iters_ratio * total_iters, 1), 3)
        warmup_lr_start = max(warmup_lr_ratio * lr, 1e-6)
        no_aug_iter = min(max(no_aug_iter_ratio * total_iters, 1), 15)
        # 使用partial函数将固定参数传给yolox_warm_cos_lr
        func = partial(yolox_warm_cos_lr, lr, min_lr, total_iters, warmup_total_iters, warmup_lr_start, no_aug_iter)
    else:
        # 否则，使用逐步衰减的策略
        # 计算每次衰减的速率
        decay_rate = (min_lr / lr) ** (1 / (step_num - 1))
        # 计算每步的衰减步长
        step_size = total_iters / step_num
        # 使用partial函数将固定参数传给step_lr
        func = partial(step_lr, lr, decay_rate, step_size)

    # 返回选择好的学习率调度函数
    return func


def set_optimizer_lr(optimizer, lr_scheduler_func, epoch):
    # 使用学习率调度函数 lr_scheduler_func，根据当前的 epoch 获取对应的学习率
    lr = lr_scheduler_func(epoch)

    # 遍历优化器的所有参数组（optimizer.param_groups 是一个包含所有参数组的列表）
    for param_group in optimizer.param_groups:
        # 将当前参数组的学习率（lr）更新为调度函数计算得到的学习率
        param_group['lr'] = lr

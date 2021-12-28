Programming Pytorch for DL
======================================

Image Classification with Pytorch
----------------------------------

Dataset
~~~~~~~~

*Training set*  Used in the training pass to update the model
*Validation set*  Used to evaluate how the model is generalizing to the problem domain
*Test set*  A final dataset that provides a final evaluation of the model's performence after training is complete

Training Loop
~~~~~~~~~~~~~~

.. code:: python

    for epoch in range(epochs):
        for batch in train_loader:
            optimizer.zero_grad()
            input, target = batch
            output = model(input)
            loss = loss_fn(output, target) # loss 函数衡量模型预测的好坏
            loss.backward()
            optimizer.step() # 优化器根据网络反向传播的梯度来更新网络参数，以降低loss函数计算值

Working on the GPU
~~~~~~~~~~~~~~~~~~

.. code:: python

    if torch.cuda.is_available():
        device = torch.device("cuda")
    else:
        device = torch.device("cpu")

    model.to(device)


Making predictions
~~~~~~~~~~~~~~~~~~

.. code:: python

    from PIL import Image

    labels = ['cat', 'fish']

    img = Image.open(FILENAME)
    img = transforms(img)
    img = img.unsqueeze(0)

    prediction = simplenet(img)
    prediction = prediction.argmax()
    print(labels[prediction])


Model saving and loading
~~~~~~~~~~~~~~~~~~~~~~~~~

**什么是状态字典**

是一个简单的python字典，其键值对是每个网络层和其对应的参数。
模型的状态字典只包含带有可学习参数的网络层(如卷积层，全连接层等)和注册的缓存(batchnorm 的 running_mean)。
优化器对象(torch.optim)同样也是有一个状态字典，包含优化器状态的信息以及使用的超参数。

.. code:: python

    # Initialize model
    model = TheModelClass()

    # Initialize optimizer
    optimizer = optim.SGD(model.parameters(), lr=0.001, momentum=0.9)

    # Print model's state_dict
    print("Model's state_dict:")
    for param_tensor in model.state_dict():
        print(param_tensor, "\t", model.state_dict()[param_tensor].size())

    # Print optimizer's state_dict
    print("Optimizer's state_dict:")
    for var_name in optimizer.state_dict():
        print(var_name, "\t", optimizer.state_dict()[var_name])


**预测时加载和保存模型**

有两种保存方法：保存状态字典，保存整个模型

*保存/加载状态字典(推荐)*

.. code:: python

    # saving
    torch.save(model.state_dict(), PATH)

    # loading
    model = TheModelClass(*args, **kwargs)
    model.load_state_dict(torch.load(PATH))
    model.eval()

通常会用 `.pt` 或 `.pth` 后缀来保存模型

*保存/加载整个模型*

.. code:: python

    # saving
    torch.save(model, PATH)

    # loading
    model = torch.load(PATH)
    model.eval()


缺点：该方法是使用pickle模块来保存整个模型，序列化后的数据是属于特定的类和指定的字典结构，
而pickle并没有保存模型类别，而是保存一个包含该类的文件路径，因此，当其他项目或者在refactor之后都可能出现错误。

**保存和加载一个通用的检查点(Checkpoint)**

当保存一个通用的checkpoint时，无论是用于继续训练还是预测，都需要保存更多的信息，如优化器的 `state_dict` ，
它包含了用于模型训练时需要更新的参数和缓存信息，还可以保存的信息包括 `epoch`, 即中断训练的批次，
最后一次训练的loss，额外的 `torch.nn.Embedding` 层等等

*saving*

.. code:: python

    torch.save({
                'epoch': epoch,
                'model_state_dict': model.state_dict(),
                'optimizer_state_dict': optimizer.state_dict(),
                'loss': loss,
                ...
                }, PATH)

*loading*

.. code:: python

    model = TheModelClass(*args, **kwargs)
    optimizer = TheOptimizerClass(*args, **kwargs)

    checkpoint = torch.load(PATH)
    model.load_state_dict(checkpoint['model_state_dict'])
    optimizer.load_state_dict(checkpoint['optimizer_state_dict'])
    epoch = checkpoint['epoch']
    loss = checkpoint['loss']

    model.train()


**采用另一个模型的参数来预热模型(Warmstaring Model)**

.. code:: python

    # saving
    torch.save(model.state_dict(), PATH)

    # loading
    modelB = TheModelBClass(*args, **kwargs)
    modelB.load_state_dict(torch.load(PATH), strict=False)


加载预训练模型的部分网络参数作为模型的初始化参数，然后可以加快模型的收敛速度。
代码如上所示， `strict=False` 表示忽略不匹配的网络层参数。


**不同设备下保存和加载模型**

*saving on GPU, loading on CPU*

.. code:: python

    # saving
    torch.save(model.state_dict(), PATH)

    # loading
    device = torch.device('cpu')
    model = TheModelClass(*args, **kwargs)
    model.load_state_dict(torch.load(PATH, map_location=device))


*saving on GPU, loading on GPU*

.. code:: python

    # saving
    torch.save(model.state_dict(), PATH)

    # loading
    device = torchh.device('cuda')
    model = TheModelClass(*args, **kwargs)
    model.load_state_dict(torch.load(PATH))
    model.to(device)



参考文档
---------

| `SAVING AND LOADING MODELS <https://pytorch.org/tutorials/beginner/saving_loading_models.html>`_
| `Everything You Need To Know About Saving Weights In PyTorch <https://towardsdatascience.com/everything-you-need-to-know-about-saving-weights-in-pytorch-572651f3f8de>`_

Use NNI on Google Colab
========================

**1. 挂载google drive**

| ``from google.colab import drive``
| ``drive.mount('/content/drive')``
| ``%cd /content/drive/My Drive``

**2. 安装需要的软件和包**

| ``!pip install nni``
| ``!wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip``
| ``!unzip ngrok-stable-linux-amd64.zip``
| ``!mkdir -p nni_repo``
| ``!git clone https://github.com/microsoft/nni.git nni_repo/nni``
| ``!pip install urllib3==1.25.10``

**3. 安装pytorch**

| ``!pip install torch==1.7.1+cu101 torchvision==0.8.2+cu101 torchaudio==0.7.2 -f https://download.pytorch.org/whl/torch_stable.html``

4. 注册 `ngrok账号 <https://dashboard.ngrok.com/get-started/setup>`_

| ``!./ngrok authtoken 1ldBaxJzGipGJver4y8lTc4IOJ9_4u3bnKohRgZRegvJA2AfF``

**5. 上传数据集和代码**

本文档以旋扭开关为例

**6. 生成训练集和测试集**

| ``%cd /content/drive/MyDrive/nni/knob``
| ``!python3 create_txt.py``

**7. 在大于1024的端口号上启动NNI，之后在相同的端口上启动ngrok**

| ``!nnictl create --config /content/drive/MyDrive/nni/knob/knob_pytorch/config.yml --port 5000 & get_ipython().system_raw('./ngrok http 5000 &``


**8. 查看公网ip**

| ``!curl -s http://localhost:4040/api/tunnels``

参考文档
---------

| `Use NNI on Google Colab <https://nni.readthedocs.io/en/stable/CommunitySharings/NNI_colab_support.html>`_


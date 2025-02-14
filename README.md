# 面向深度学习表格数据模型的回归缺陷检测和缓解研究项目

## 项目介绍
本仓库包含了面向深度学习表格数据模型的回归缺陷检测和缓解研究项目的代码。该项目旨在解决深度学习表格数据模型中可能出现的回归缺陷问题，通过一系列的算法和技术来检测和缓解这些缺陷，以提高模型的性能和可靠性。

## 项目结构
本项目的主要目录结构如下：
- `DTFuzz/`
  - `dtfuzz/`：包含主要的代码模块和功能实现。
    - `autoencoder/`：自编码器相关代码。
      - `ctgan/`：CTGAN（条件生成对抗网络）相关代码。
        - `ctgan.py`：CTGAN 模型的实现。
        - `data.py`：数据处理相关函数，如 `read_csv` 和 `read_tsv`。
    - `src/`：核心源代码目录。
      - `DrFuzz.py`：DrFuzz 相关功能实现。
      - `experiment_builder.py`：实验构建相关代码。
    - `utils/`：工具函数目录。
      - `param_util.py`：参数处理相关函数，如 `load_params` 和 `get_params`。
      - `logger.py`：日志记录相关代码。
      - `struct_util.py`：数据结构处理相关代码。
      - `expect_grad_ops_util.py`：期望梯度操作相关代码。
  - `TRFMitigator/`：TRF 缓解器相关代码。
    - `src/`：核心源代码目录。
      - `DrFuzz.py`：DrFuzz 相关功能实现。
    - `test.py`：测试代码。
    - `utils/`：工具函数目录。
      - `param_util.py`：参数处理相关函数，如 `load_params` 和 `get_params`。
  - `params/`：参数配置文件目录。
    - `change.py`：参数配置文件。

## 安装与依赖
本项目基于 Python 开发，主要依赖以下库：
- `tensorflow`：深度学习框架。
- `numpy`：数值计算库。
- `pandas`：数据处理库。
- `scipy`：科学计算库。
- `argparse`：命令行参数解析库。
- `importlib`：模块导入库。

你可以使用以下命令安装所需的依赖：
~~~
conda create -n Feaproct python=3.8
conda install numpy=1.21.5
conda install tensorflow==2.3.0
conda install matplotlib
conda install pandas
pip install minepy
conda install pytorch==1.13.0 torchvision==0.14.0 torchaudio==0.13.0 pytorch-cuda=11.7 -c pytorch -c nvidia
conda install -c conda-forge rdt
~~~
# 使用方法
## 运行测试代码
### Running DTFuzz

~~~
python main.py --dataset mobile_price --model mobile_price --params_set Dnn mobilePrice change drfuzz --output_name price_range --dataset_col sc_h-talk_time-touch_screen-battery_power-four_g-three_g --terminate_type time --choose_col_type random --time 360 --update_col_num 3
~~~

main.py contains all the configurations for our experimentation.

You can refer to Drfuzz for configuration. 

`--update_col_num` refers to the number of columns selected

`--output_name` refers to the label column of the dataset

`--dataset_col` refers to scenarios

`--choose_col_type` refers to the selection strategy for the number of columns selected

### Running TRFMitigator

~~~
python test.py --dataset mobile_price --model mobile_price --num_classes 4 --dataset_col sc_h-talk_time-touch_screen-battery_power-four_g-three_g --fix_type eg_discrt_fea_sel_1 --rf  --need_eg --need_fea_Sel
~~~

从 RQ2 消融实验中获得的模型分别是`eg_1`, `eg_2`, `eg_3`, `discrt_fea_sel_1`, `discrt_fea_sel_2`, and `discrt_fea_sel_3`
~~~
python test.py --dataset mobile_price --model mobile_price --num_classes 4 --dataset_col sc_h-talk_time-touch_screen-battery_power-four_g-three_g --fix_type eg_1 --rf --need_eg
~~~

~~~
python test.py --dataset mobile_price --model mobile_price --num_classes 4 --dataset_col sc_h-talk_time-touch_screen-battery_power-four_g-three_g --fix_type discrt_fea_sel_1 --rf  --need_fea_Sel
~~~

----- 
如果研究人员需要对我们的方法进行实验，他们可以简单地使用以下命令运行`TRFMitigator`.

~~~
python main.py --dataset <datasetname> --model <datasetname> --num_classes <number of class> --dataset_col <scenarios> --fix_type eg_discrt_fea_sel --need_eg --need_fea_Sel --lamb 0.5
~~~


main.py 文件包含了我们实验的所有配置。

`--num_classes` 指的是数据集的类别数量。

`--dataset_col` 指的是实验场景。

`--fix_type` 指的是保存路径。

`--rf` 指的是是否使用回归误差。如果为 True，你可以使用这些参数来计算回归误差修复率。

`--need_eg` 指的是是否使用期望梯度。

`--need_fea_Sel` 指的是是否使用特征选择。

同样，你可以使用我们提供的 shell 脚本来进行实验。


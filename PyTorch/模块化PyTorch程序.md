工程的组织结构如下。
工程名称为 `go_modular`，子文件夹`go_modular`包含：
* `data_setup.py` - 预处理和下载数据。
* `engine.py` - 各种train和eval函数。
* `model_builder.py` - 创建模型
* `train.py` - 利用其他文件训练目标 PyTorch 模型。
* `utils.py` - 一些通用的辅助函数。

文件夹`data`是用于训练的数据。
文件夹`models`用于保存模型参数。
```
going_modular/
├── going_modular/
│   ├── data_setup.py
│   ├── engine.py
│   ├── model_builder.py
│   ├── train.py
│   └── utils.py
├── models/
│   ├── 05_going_modular_cell_mode_tinyvgg_model.pth
│   └── 05_going_modular_script_mode_tinyvgg_model.pth
└── data/
    └── pizza_steak_sushi/
        ├── train/
        │   ├── pizza/
        │   │   ├── image01.jpeg
        │   │   └── ...
        │   ├── steak/
        │   └── sushi/
        └── test/
            ├── pizza/
            ├── steak/
            └── sushi/
```
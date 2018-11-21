## Python中遍历文件夹的所有文件os.walk

```
在flask里面使用钩子函数删除CODE_DIR里面的所有图片
@user_blueprint.before_request
def before_request():
    code_dir=CODE_DIR
    for root, dirs, files in os.walk(code_dir):
        for file in files:
            os.remove(code_dir + '/' + file)
```

| roots | 代表需要遍历的根文件夹                   |
| ----- | ---------------------------------------- |
| root  | 表示正在遍历的文件夹的**名字**（根/子）  |
| dirs  | 记录正在遍历的文件夹下的**子文件夹**集合 |
| files | 记录正在遍历的文件夹中的**文件**集合     |
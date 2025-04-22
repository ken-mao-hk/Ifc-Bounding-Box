IFC Bounding Box Calculator
项目描述
一个用于计算 IFC（Industry Foundation Classes）模型中建筑构件（如墙、柱等）边界框（Bounding Box）和三维尺寸的工具。支持输出构件在局部坐标系和全局坐标系下的边界框顶点坐标，并正确处理构件旋转后的尺寸定义（解决长度、高度交换问题）。
核心功能
IFC 文件解析：读取 IFC 模型并提取构件几何信息。
边界框计算：
计算构件在局部坐标系下的原始边界框尺寸（长、宽、高）。
自动处理构件旋转，确保尺寸定义符合工程语义（长度 / 宽度为水平方向，高度为垂直方向）。
坐标转换：
输出局部坐标系下的边界框顶点坐标。
转换并输出全局坐标系下的边界框顶点坐标（考虑 IFC 模型的位置和旋转）。
精度控制：结果保留 3 位小数，避免浮点数误差。
安装要求
依赖环境
Python 3.9+
依赖库（通过 pip 安装）：
bash
pip install ifcopenshell numpy

文件结构
plaintext
.
├── ifc_bbox_calculator.py  # 核心代码文件
├── requirements.txt        # 依赖清单
└── README.md               # 项目说明
使用方法
1. 代码导入
python
from ifc_bbox_calculator import get_objects_bounding_boxes
2. 主函数参数
python
get_objects_bounding_boxes(
    ifc_file_path: str,       # IFC文件路径（必填）
    ifc_type: str = "IfcWall" # 目标构件类型（可选，默认值：墙）
) -> dict
3. 示例运行
python
if __name__ == "__main__":
    file_path = "path/to/your_model.ifc"
    bboxes = get_objects_bounding_boxes(file_path)
    
    for guid, data in bboxes.items():
        print(f"构件类型: {data['type']}")
        print(f"尺寸 (LxWxH): {data['dimensions']['L']} x {data['dimensions']['W']} x {data['dimensions']['H']}")
        print("局部坐标系顶点坐标:")
        for i, vertex in enumerate(data['coordinates']['local']):
            print(f"顶点 {i}: {vertex}")
        print("全局坐标系顶点坐标:")
        for i, vertex in enumerate(data['coordinates']['global']):
            print(f"顶点 {i}: {vertex}")
输出格式说明
结果字典结构
python
{
    "type": "构件类型 (如 IfcWall)",          # IFC类型
    "dimensions": {
        "L": float,  # 水平方向最长边（长度）
        "W": float,  # 水平方向最短边（宽度）
        "H": float   # 垂直方向高度
    },
    "coordinates": {
        "local": list[list[float]],  # 局部坐标系8个顶点坐标（x, y, z）
        "global": list[list[float]]  # 全局坐标系8个顶点坐标（x, y, z）
    }
}
贡献与反馈
如果您发现问题或有优化建议，欢迎通过以下方式反馈：
在 GitHub 仓库提交 Issue
发送邮件至 your-email@example.com
项目开源协议：MIT License
许可证
plaintext
MIT License

Copyright (c) 2023 Your Name

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
使用说明
将代码文件、requirements.txt 和 README.md 保存到同一目录
通过 GitHub 桌面客户端或命令行上传至您的仓库
在仓库设置中配置 README 显示即可
如果需要进一步调整格式或补充内容，请随时告诉我

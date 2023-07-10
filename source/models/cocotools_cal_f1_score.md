# 使用cocotools进行F1-Score评估

> [参考链接](https://cocodataset.org/#format-results)
>
> [pycocoEvalDemo](https://github.com/cocodataset/cocoapi/blob/master/PythonAPI/pycocoEvalDemo.ipynb)

## 1. Object Detection

对于边界框的检测，请使用以下格式：

```json
[{
"image_id" : int, "category_id" : int, "bbox" : [x,y,width,height], "score" : float,
}]
```

## 2. 计算f1_score

**F1-Score: Integrating Precision and Recall, the formula is: F1-Score = (2 * Precision * Recall) / (Precision + Recall)**

```python
import argparse
from pycocotools.coco import COCO
from pycocotools.cocoeval import COCOeval
    
def calculate_metrics(anno_file, pred_file):
    # 加载ground truth数据
    coco_gt = COCO(anno_file)
    # 加载预测数据
    coco_dt = coco_gt.loadRes(pred_file)

    # 使用COCOeval工具进行评估
    cocoEval = COCOeval(coco_gt, coco_dt, iouType='bbox')  # 更改iouType为你需要的类型
    cocoEval.evaluate()
    cocoEval.accumulate()
    cocoEval.summarize()

    precision = round(cocoEval.stats[1]*100, 3)  # Precision at IoU
    recall = round(cocoEval.stats[8]*100, 3)     # Recall at IoU
    ap = round(cocoEval.stats[2]*100, 3)         # AP at IoU
    # 计算F1-Score
    f1_score = round(2 * (precision * recall) / (precision + recall),3)

    print(f" F1-Score: {f1_score}, Precision: {precision}, Recall: {recall}, AP: {ap}")

if __name__ == '__main__':
    parser = argparse.ArgumentParser(description='Calculate F1-score, precision, recall and AP for COCO dataset')
    parser.add_argument('--anno', type=str, required=True, help='Path to the annotation json file')
    parser.add_argument('--pred', type=str, required=True, help='Path to the prediction json file')

    args = parser.parse_args()

    calculate_metrics(args.anno, args.pred)

```


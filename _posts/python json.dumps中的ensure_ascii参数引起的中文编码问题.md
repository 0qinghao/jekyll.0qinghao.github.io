json.dumps序列化时对中文默认使用的ascii编码，想输出真正的中文需要指定ensure_ascii=False。

```python
result_str = json.dumps(result, ensure_ascii = False)
```
# Regex

{% embed url="https://refrf.shreyasminocha.me/" %}

Python

```python
import re
domains = [
  'abc.com',
  'cnn.com',
]

domain_list = [re.escape(i) for i in domains]
reg = '|'.join(domain_list)
reg
'abc\\.com|cnn\\.com'

# pyspark
df2 = df1.filter(df.col1.rlike(reg))

df3 = df1.filter('col1 not rlike "abc\\.com|cnn\\.com"')
```




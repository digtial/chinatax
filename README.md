# chinatax
### 国税局验证码识别，发票查验
### 你不是喜欢反编译吗？现在低价出售识别率92%的模型，100元一套，联系QQ：3421366528。你敢买我就敢卖。限时抢购。想要识别率97%的模型价格较高，手头没钱的建议购买92%的模型，自己可以爬一些数据，尝试采用更好的模型达到更高的识别率。

### 联系QQ：3421366528。
### 说明：某个诋毁我的人我也劝你耗子尾汁，你的博客被举报就来说是我干的，那我的博客就在昨天（2020年12月27日半夜11点多）被举报可不可以说是你举报的呢？如果我要举报干啥不一开始就举报，何必等到现在？试用版的准确率当然不会放最好的模型进去，虽然放的不是最好的我放的识别率也有90%，因为我知道pyinstaller打包的软件有可能会被反编译。至于模型也是旧的模型结构，随你开心就好。还是很高兴你第一时间想到是我，说明我的识别模型确实对你的暴利收入造成威胁，马上2021年了，祝你元旦快乐。对了，我压根没使用你那个java版的生成器，我的试用版软件里面也没有放生成器的代码，自己在那诋毁别人，恰恰说明自己心虚。妈的举报老子的博客导致无法登陆，还在那大放厥词，触碰到你的利益了是吧？你咋不80万，800万一套呢？

***请使用post，get返回网页指导。***

**验证码识别**
Python请求示例代码如下：
```python
import base64
import requests

with open('./test02.png', 'rb') as f:
    img_bytes = f.read()

img_base64 = base64.b64encode(img_bytes)
# '00' 黑色 '01' 红色 '02' 黄色 '03' 蓝色
data = {'image': str(img_base64, 'utf-8'), 'key': '03'}
result = requests.post('http://42.192.80.101:8808/captcha', json=data)
或
result = requests.post('http://47.99.174.98:8808/captcha', json=data)
# 返回json格式数据{"code": "7RT"}
print(result.json())
```


JAVA请求代码如下：
```java
import java.io.*;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Base64;


public class CaptchaRecognize {
    static String captcha_url = "http://42.192.80.101:8808/captcha";

    public static String getBase64(String imgFile) {
        InputStream inputStream = null;
        byte[] data = null;
        try {
            inputStream = new FileInputStream(imgFile);
            data = new byte[inputStream.available()];
            inputStream.read(data);
            inputStream.close();

        } catch (IOException e) {
            e.printStackTrace();
        }
        Base64.Encoder encoder = Base64.getEncoder();
        assert data != null;
        return encoder.encodeToString(data);
    }

    public static void captchaPost() throws IOException {
        String imgBase64 = getBase64("./imgs/img001.jpg");
        String data = "{" + "\"image\":" + "\"" + imgBase64 + "\"" + "," + "\"key\":" + "\"03\"" + "}";
        URL url = new URL(captcha_url);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setDoOutput(true);
        conn.setDoInput(true);
        conn.setRequestProperty("Content-Type", "application/json");
        conn.setRequestProperty("Accept", "application/json");
        OutputStreamWriter out = new OutputStreamWriter(conn.getOutputStream());
        out.write(data);
        out.flush();
        out.close();
        BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream(), "UTF-8"));
        String line;
        while ((line = in.readLine()) != null) {
            System.out.println(line); // {"code": "Y2W"}
        }
    }

    public static void main(String[] args) throws IOException {
        captchaPost();
    }
}

```


### 发票查验
查验规则同国税局官网，参数为发票代码、发票号码、开票日期、校验码。
下面两个地址的响应速度不一样。
```python
import requests
data = {'fpdm': '042001900211', 'fphm': '88825677', 'rq': '20200510', 'jym': '240140'}
result = requests.post('http://42.192.80.101:8808/fp', json=data)
或
result = requests.post('http://47.99.174.98:8808/fp', json=data)
print(result.json())
```
返回结果如下，目前正在做发票版面格式化，做完整以后更新接口，4核服务器发票查验平均时间为4秒，1核服务器发票查验平均时间为8秒。

{'key1': '001', 'key2': '0≡20200510≡武汉京东金德贸易有限公司≡9142011733356690XY≡武汉市新洲区阳逻经济开发区红岗村（创业服务中心11楼） 027-84295702≡交通银行武汉汉阳支行 421865098018800012527≡个人≡≡≡≡83659874142294240140≡8.34≡72.5≡≡661619940897≡64.16≡0≡≡', 'key3': '*方便食品*好欢螺 螺蛳粉（水煮型）广西柳州特产方便面粉速食米线 袋装400g█螺蛳粉400g█袋█13.0█13.18584071█65.93█5.00000000█8.57██1030203030000000000≡*方便食品*好欢螺 螺蛳粉（水煮型）广西柳州特产方便面粉速食米线 袋装400g███13.0██-1.77██-0.23██1030203030000000000', 'key4': '订单号:119950827814', 'key5': '1'}

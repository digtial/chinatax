# chinatax
# 国税总局增值税发票查验平台验证码识别
## CSDN地址：https://blog.csdn.net/Open_CV_NLP_New
## 博客园地址：https://www.cnblogs.com/open-cv-nlp/p/14269675.html
### 联系QQ：3421366528。
### 有段时间没看github了，新的一年继续干。


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
    static String captcha_url = "http://47.99.174.98:8808/captcha";

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

PS:1.用真实发票查验，下面的data已经脱敏。2.查验专票jym填写不含税金额。
```python
import requests
data = {'fpdm': '044001505121', 'fphm': '38507145', 'rq': '20180926', 'jym': '865375'}
result = requests.post('http://47.99.174.98:8808/fp', json=data)
print(result.json())
```

{'key1': '001', 'key2': '0≡20180916≡珠海市魅族科技有限公司≡91********39363≡珠海市科技创新海岸魅族科技楼07566116288≡中国工商银行股份有限公司珠海唐家支行 2002021719100112933≡######≡ ≡≡≡45239907091899865275≡454.90≡3298.00≡≡661616290243≡2843.10≡0≡≡', 'key3': '*移动通信设备*M882Q-魅族16国内全网通公开版远山白8+128GB███0.160█2843.10000000█2843.10█1.00000000█454.90██1090505000000000000', 'key4': '订单号:123456789051048928121 软件:魅族Flyme智能手机操作软件V7.1.1;数量:1;不含税金额:68.97;税额:11.03', 'key5': '1', 'key11': 'o'}


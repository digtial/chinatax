# chinatax
# 国家增值税发票查验平台验证码识别
## CSDN地址：https://blog.csdn.net/Open_CV_NLP_New
## 博客园地址：https://www.cnblogs.com/open-cv-nlp/p/14269675.html
### 联系QQ：3421366528。
### 由于某些人的举报导致我的csdn账号无法登陆，然而我并没有第一时间在网上发帖说是他举报的，其实他之前加过我好友，稍后看下图，后来发现他的博客也被举报了，但是他第一时间在网上发帖说我干的，我不知道是有第三个人举报让我们互掐，坐享渔翁之利，还是那个人自编自导的一场好戏，看评论说他今年考研，可能考完了要对我下手了。他还说我是苦肉计，我问候你全家好吧。其实国税局的验证码识别这块确实做到识别率达到97%没几个，我确实做到了，我暂且相信他也做到了。其实是由于我导师前段时间接了一个税务方面的小项目派给我和师弟做的，所以大家之前并未看到我发表的关于国税局验证码识别的接口，在我做出来之前，我看了一下网上都是那个人的帖子，谁先做出来谁先垄断嘛，但是导师的任务又不能放弃，所以经过研究自己写了生成器代码，他说我看他的Java生成器，然而并没有，如果你的生成器真的有效，别人早就有识别率很高的模型了，何必轮到我呢，那个代码只是相对比较接近真实的验证码，但是肉眼可见的差别还是很大的。再说说那个试用版的打包软件，既然是试用版肯定要加个试用期限，但是里面的pb模型并没有放识别率最好的那个，而是放了一个识别率中等的模型，大概90%，那个人觉得自己反编译了，觉得自己挺牛🍺，还把通过pyinstaller打包前的Python代码公布出来，说是定时炸弹，我再次问候你全家，没看见写的是试用版吗？眼睛不想要可以捐了，省的浪费。
### 然后好了，他的博客已经恢复正常了，然而我的账号没了，我不管是不是你举报的，至少我没有第一时间发帖说是你举报的，说明你很在意别人做的识别模型。还说别人不讲武德，你自己才是最不讲武德的。
### 为什么那个人要不停的发帖，显示自己的水平，就是因为我的价格便宜几百块钱一套，而他8万一套，他心里急呀，因为他垄断市场好几年了，这块肥肉不能丢了呀，继续发吧。
### 看他的发的消息截图：
![Image text](https://github.com/digtial/chinatax/blob/main/%E6%B6%88%E6%81%AF%E6%88%AA%E5%9B%BE.JPG)
  
  
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
42开头的服务器即将到期，请使用如下的地址。
```python
import requests
data = {'fpdm': '044001505121', 'fphm': '38507145', 'rq': '20180926', 'jym': '865375'}
result = requests.post('http://47.99.174.98:8808/fp', json=data)
print(result.json())
```

{'key1': '001', 'key2': '0≡20180916≡珠海市魅族科技有限公司≡914404007480139363≡珠海市科技创新海岸魅族科技楼07566116288≡中国工商银行股份有限公司珠海唐家支行 2002021719100112933≡######≡ ≡≡≡44829907091899865275≡454.90≡3298.00≡≡661616290243≡2843.10≡0≡≡', 'key3': '*移动通信设备*M882Q-魅族16国内全网通公开版远山白8+128GB███0.160█2843.10000000█2843.10█1.00000000█454.90██1090505000000000000', 'key4': '订单号:22082501351048928121 软件:魅族Flyme智能手机操作软件V7.1.1;数量:1;不含税金额:68.97;税额:11.03', 'key5': '1', 'key11': 'o'}


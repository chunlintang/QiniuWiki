### ###七牛云fetch抓取远程文件上传

(1),请求
```
POST /fetch/aHR0cDovL3Fpbml1LmNvbS9pbmRleC5odG1s/to/bmV3ZG9jczpmaW5kLm1hbi50eHQ= HTTP/1.1
User-Agent: curl/7.30.0
Host: iovip.qbox.me
Accept: */*
Authorization: QBox u8WqmQu1jH21kxpIQmo2LqntzugM1VoHE9_pozCU:2LJIG...
```
(2),代码示例
```
// 七牛配置
> app.config
'qiniu' => [
        'bucket' => 'mybucket',
        'SecretKey' => '', //your SecretKey
        'AccessKey' => '', //your AccessKey
        'domain' => '', //your Host
    ],
/**
*  获取远程文件的地址$url,从远程拉取下载上传到七牛
*  @params $url
*  @return string
*/
public function qiniuFetch($url)
{
        $encodedURL = str_replace(array('+', '/'), array('-', '_'), base64_encode($url));
        $encodedEntryURI = str_replace(array('+', '/'), array('-', '_'), base64_encode(Config::get('app.qiniu.bucket')));
        $url = '/fetch/' . $encodedURL . '/to/' . $encodedEntryURI;
        $sign = hash_hmac('sha1', $url . "\n", Config::get('app.qiniu.SecretKey'), true);
        $token = Config::get('app.qiniu.AccessKey') . ':' . str_replace(array('+', '/'), array('-', '_'), base64_encode($sign));
        $header = array('Host: iovip.qbox.me', 'Content-Type:application/x-www-form-urlencoded', 'Authorization: QBox ' . $token);
        $curl = curl_init();
        curl_setopt($curl, CURLOPT_URL, trim('http://iovip.qbox.me' . $url, '\n'));
        curl_setopt($curl, CURLOPT_HTTPHEADER, $header);
        curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($curl, CURLOPT_POSTFIELDS, "");
        $result = json_decode(curl_exec($curl), true);
        curl_close($curl);
        //返回一个七牛的url
        return Config::get('app.qiniu.domain') . $result['key'];
}
```
<p style="color:red">＊注：对于一些音频文件需做转码处理</p>

### ###七牛云上传本地图片到私有空间
(1),将远程文件下载到本地
```
$urls = [
        'https://www.mantis.me/uploads/1.jpg',
        'https://www.mantis.me/uploads/2.jpg',
        'https://www.mantis.me/uploads/3.jpg',
        'https://www.mantis.me/uploads/4.jpg',
        'https://www.mantis.me/uploads/5.jpg'
]
```
//创建本地文件夹
```
$dir = "/uploads";
mkdir($dir);
$curl = $text = array();
foreach ($urls as $k => $v) {
        if (!empty($v) && preg_match("~^http~i", $v->MonitorFilename)) {
                $nurl[$k] = trim(str_replace(' ', "%20", $v->MonitorFilename));
                $curl[$k] = curl_init($nurl[$k]);
                curl_setopt($curl[$k], CURLOPT_IPRESOLVE, CURL_IPRESOLVE_V4);
                curl_setopt($curl[$k], CURLOPT_RETURNTRANSFER, 1);
                curl_setopt($curl[$k], CURLOPT_HEADER, 0);
                curl_setopt($curl[$k], CURLOPT_CONNECTTIMEOUT, 20);
                if (!isset($handle)) {
                    $handle = curl_multi_init();
                }
        curl_multi_add_handle($handle, $curl[$k]);
        }
continue;
}
$active = null;
do {
        $mrc = @curl_multi_exec($handle, $active);
} while ($mrc == CURLM_CALL_MULTI_PERFORM);
while ($active && $mrc == CURLM_OK) {
        if (curl_multi_select($handle) != -1) {
                do {
                    $mrc = curl_multi_exec($handle, $active);
                } while ($mrc == CURLM_CALL_MULTI_PERFORM);
            }
        }
        foreach ($curl as $k => $v) {
            if (curl_error($curl[$k]) == "") {
                //将文件名命名为'id-原文件名'的形式
                $fname[$k] = $urls[$k]->id . '-' . pathinfo($urls[$k]->MonitorFilename, PATHINFO_BASENAME);
                $text[$k] = (string)curl_multi_getcontent($curl[$k]);
                $filedir[$k] = $dir . '/' . $fname[$k];
                if (file_put_contents($filedir[$k], $text[$k])) {
                    $filepath[$k] = $path . $fname[$k];
                }
            }
            curl_multi_remove_handle($handle, $curl[$k]);
            curl_close($curl[$k]);
        }
curl_multi_close($handle);

// 将下载的本地文件上传到七牛云
$filenames = scandir($dir);
foreach ($filenames as $k => $v) {
        if ($v != "." && $v != "..") {
                // 需要填写 Access Key 和 Secret Key
                $accessKey = '';
                $secretKey = '';
                $domain = '';
                // 构建鉴权对象
                $auth = new \Qiniu\Auth($accessKey, $secretKey);
                // 要上传的空间
                $bucket = ;
                // 生成上传 Token
                $token = $auth->uploadToken($bucket);
                // 要上传文件的本地路径
                $filePath = $dir . $v;
                // 上传到七牛后保存的文件名
                $key = $v;
                // 初始化 UploadManager 对象并进行文件的上传
                $uploadMgr = new UploadManager();
                // 调用 UploadManager 的 putFile 方法进行文件的上传
                list($ret, $err) = $uploadMgr->putFile($token, $key, $filePath);
                if (is_null($err)) {
                    $baseUrl = $domain . $ret['key'];
                    // 生成私有下载链接/有效日期2年
                    $privateurl = $auth->privateDownloadUrl($baseUrl, $expires = 3600 * 24 * 365 * 2);
                    echo $privateurl;
                } else {
                    Log::error($err);
                }
        }
}
```

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
＊注：对于一些音频文件需做转码处理

<?php
require "./curl/curl.php";
header("Content-type: text/html; charset=utf-8");
$curl = new Curl;
$vid = 'e0020786eop';
$url="http://vv.video.qq.com/getinfo?otype=json&vid={$vid}";
$post_data=array();
$post_data['platform'] = 11;
$post_data['newplatform']=11;
$post_data['dtype']=3;

$result = $curl->post($url,$post_data);
$json_str = $result->body;
$pos = strlen("QZOutputJson=");
$str = substr($json_str,$pos);
$str  = rtrim($str,';');
$arr = json_decode($str,1);

echo "<pre>";
print_r($arr);
echo "</pre>";
$vid = $arr['vl']['vi'][0]['lnk']; //vid重造
$url_view = $arr['vl']['vi'][0]['ul']['ui'][0]['url'];
$format = $arr['fl']['fi'][3]['id'];
$type = $arr['fl']['fi'][3]['name'];
$format = strval($format);
var_dump($format,$type);
$spec_num = substr($format,-3);
$duration = $arr['vl']['vi'][0]['td'];
$vedio_num = floor($duration/300);
$range = range(1,$vedio_num);

foreach($range as $num){
$filename= $vid.".p".$spec_num.".{$num}.mp4";
$url_key ="http://vv.video.qq.com/getkey?format={$format}&type={$type}&vid={$vid}&filename={$filename}";

$ret = $curl->post("http://vv.video.qq.com/getkey",array("format"=>$format,"type"=>$type,"vid"=>$vid,"filename"=>$filename));

$sXML = download_page($url_key);
$oXML = new SimpleXMLElement($sXML);
$vkey = $oXML->key;

var_dump(($url_view.$filename."?type=mp4&vkey={$vkey}&fmt={$type}"));
}
function download_page($path){
        $ch = curl_init();
        curl_setopt($ch, CURLOPT_URL,$path);
        curl_setopt($ch, CURLOPT_FAILONERROR,1);
        @curl_setopt($ch, CURLOPT_FOLLOWLOCATION,1);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER,1);
        curl_setopt($ch, CURLOPT_TIMEOUT, 15);
        $retValue = curl_exec($ch);
        curl_close($ch);
        return $retValue;
}

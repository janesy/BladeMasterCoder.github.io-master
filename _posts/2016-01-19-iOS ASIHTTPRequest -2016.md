---
layout: post
title: ASIHTTPRequest文件下载带进度显示
category: 杂记
tags: ios 
keywords: 进度追踪，ASIHTTPRequest，文件下载
description: 
---

>   文件下载的方式有很多种，系统自带的NSURLConnection请求，第三方ASI和AFNetWorking，今天我们用ASI来实现文件下载和进度追踪


## 下载之前先判断本地文件是否存在，存在就直接分享，不存在就下载
if([self isFileDownloaded])
{
[_delegate click:_currentType];
}
else
{
[self startDownload];
}

##判断文件是否存在
-(BOOL)isFileDownloaded
{
//查看本地文件路径下是否存在文件
NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
NSString *path = [paths lastObject];
NSString *filePath=[path stringByAppendingPathComponent:[NSString stringWithFormat:@"%@/%@",self.itemModel.fileID,self.itemModel.fileName]];
NSFileManager *fileManager = [NSFileManager defaultManager];
if(![fileManager fileExistsAtPath:filePath]) //如果不存在
{
return NO;
}else
{
return YES;
}

return NO;
}


##不存在，则用ASI开始文件下载
-(void)requestFile
{
//开始下载文件
NSString * path=[ NSSearchPathForDirectoriesInDomains ( NSDocumentDirectory , NSUserDomainMask , YES ) objectAtIndex : 0 ];
NSString *patientPhotoFolder = [path stringByAppendingPathComponent:[NSString stringWithFormat:@"%@/%@",self.itemModel.fileID,self.itemModel.fileName]];

if (![[NSFileManager defaultManager] fileExistsAtPath:patientPhotoFolder]) {
[[NSFileManager defaultManager] createDirectoryAtPath:[patientPhotoFolder stringByDeletingLastPathComponent] withIntermediateDirectories:YES attributes:nil error:nil];
}
if(self.itemModel.fileSize==0)
{
[indicator stopAnimating];
[_myAlertView dismissWithClickedButtonIndex:0 animated:NO];
[[NSFileManager defaultManager] createFileAtPath:patientPhotoFolder contents:nil attributes:nil];
[self performSelector:@selector(nextMove) withObject:nil afterDelay:0.5];
return;
}

NSURL *url = [ NSURL URLWithString : [NSString stringWithFormat:@"https://moadisk.oa.tencent.com/preview/download/%@",self.itemModel.tag] ];
ASIHTTPRequest *request = [ ASIHTTPRequest requestWithURL :url];
self.currentRequest=request;
[_currentRequest addRequestHeader:@"Key" value:[[MOAAdLoginModel sharedInstance] ckey]];
[_currentRequest setDownloadDestinationPath:patientPhotoFolder];
[_currentRequest setDownloadProgressDelegate:_progress];
[_currentRequest setTimeOutSeconds:30];
[_currentRequest setDelegate:self];
[_currentRequest startAsynchronous ];
}


##实现ASI的代理ASIHTTPRequestDelegate，复写其代理方法进行进度计算
-(void)request:(ASIHTTPRequest *)request didReceiveResponseHeaders:(NSDictionary *)responseHeaders{
_progress.totalSize = request.contentLength/(1024.0*1024.0);
NSLog(@"---%f",_progress.totalSize);

}

-(void)request:(ASIHTTPRequest *)request didReceiveResponseHeaders:(NSDictionary *)responseHeaders{
_progress.totalSize = request.contentLength/(1024.0*1024.0);
NSLog(@"---%f",_progress.totalSize);

}



-(void)requestFinished:(ASIHTTPRequest *)request
{
[indicator stopAnimating];
[_myAlertView dismissWithClickedButtonIndex:0 animated:NO];
[self performSelector:@selector(nextMove) withObject:nil afterDelay:0.5];
}
}

-(void)requestFailed:(ASIHTTPRequest *)request
{
_retryTime=0;
if(_isCancel) return;
[_myAlertView dismissWithClickedButtonIndex:0 animated:NO];
UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"" message:@"提取文件失败，是否重试？" delegate:self cancelButtonTitle:@"取消" otherButtonTitles:@"重试", nil];
alertView.tag=1;
[alertView show];
}


##其中进度条显示
-(void)startDownload
{

_myAlertView = [[UIAlertView alloc] initWithTitle:nil message:nil
delegate:self
cancelButtonTitle:@"取消"
otherButtonTitles:nil, nil];
_myAlertView.tag=0;
UIView *backView = [[UIView alloc]initWithFrame:CGRectMake(0,0,270,110)];
backView.backgroundColor=[UIColor clearColor];
indicator = [[UIActivityIndicatorView alloc] initWithActivityIndicatorStyle:UIActivityIndicatorViewStyleGray];
indicator.frame=CGRectMake(50, 15, 37, 37);
indicator.center=backView.center;
[backView addSubview:indicator];
[indicator startAnimating];
_progress = [[MOAProgressIndicator alloc] initWithFrame:CGRectMake(0, 5, 270, 44)];
[backView addSubview:_progress];
}


##进度由双精度型转为百分比的代码
-(void)setProgress:(float)progress{
[_progressView setProgress:progress];


//进度换算成百分比
CFLocaleRef currentLocale = CFLocaleCopyCurrent();

CFNumberFormatterRef numberFormatter = CFNumberFormatterCreate(NULL, currentLocale, kCFNumberFormatterPercentStyle);

CFNumberRef number = CFNumberCreate(NULL, kCFNumberFloatType, &progress);

CFStringRef numberString = CFNumberFormatterCreateStringWithNumber(NULL, numberFormatter, number);

NSLog(@"－－－－－%@",numberString);
_label.text = [NSString stringWithFormat:@"提取中.....%@",numberString];


}

    
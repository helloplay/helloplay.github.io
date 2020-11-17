---
layout: post
title: S3 및 CloudFront 에서 사용 중인 SSL 인증서 교체 관련 이슈 정리
subtitle: 
tags: [s3,cloudfront,amazon trust services]
comments: true
published: true
---

## S3 및 CloudFront 에서 사용 중인 SSL 인증서 교체 관련 이슈 정리
- 2021 년 3 월 23 일 시작
- S3 및 CloudFront 에서 사용되는
- 인증서의 인증기관(CA)이 DigiCert에서 Amazon Trust Services 로 자동으로 변경될 예정

- 아마존의 루트 인증서는 사설 CA(인증기관) 이지만 대부분의 OS 와 브라우저(크롬, 익스, 엣지 등) 등에서 자동으로 신뢰되게끔 제공함


## 신뢰되는 인증서인지 확인하는 방법
- ATS 가이드 내, 인증서 고유 이름을 기준으로
OS, 브라우저 등에서 지원 가능한 인증서를 대조해 보면 된다.

- ATS 가이드 : [https://aws.amazon.com/ko/blogs/security/how-to-prepare-for-aws-move-to-its-own-certificate-authority/](https://aws.amazon.com/ko/blogs/security/how-to-prepare-for-aws-move-to-its-own-certificate-authority/)

- ios 12 지원 인증서 리스트 : [https://support.apple.com/ko-kr/HT209144](https://support.apple.com/ko-kr/HT209144)
- 이 외 os 버전 리스트 : [https://support.apple.com/ko-kr/HT204132](https://support.apple.com/ko-kr/HT204132)

- aos 지원 인증서 리스트 : [https://android.googlesource.com/platform/system/ca-certificates/+/master/files/](https://android.googlesource.com/platform/system/ca-certificates/+/master/files/)
- 설정 > 보안 > 기타 보안 > 인증서 확인 메뉴를 통해 디바이스에서 직접 확인도 가능하다.


## Amazon Trust Services 인증서 테스트
### 정상 여부 테스트 사이트
- [https://s3-ats-migration-test.s3.eu-west-3.amazonaws.com/test.jpg](https://s3-ats-migration-test.s3.eu-west-3.amazonaws.com/test.jpg)
- [https://good.sca1a.amazontrust.com/](https://good.sca1a.amazontrust.com/)
- [https://good.sca2a.amazontrust.com/](https://good.sca2a.amazontrust.com/)
- [https://good.sca3a.amazontrust.com/](https://good.sca3a.amazontrust.com/)
- [https://good.sca4a.amazontrust.com/](https://good.sca4a.amazontrust.com/)
- [https://good.sca0a.amazontrust.com/](https://good.sca0a.amazontrust.com/)

### 비정상 케이스에 대한 예시
- [https://untrusted-root.badssl.com/](https://untrusted-root.badssl.com/)

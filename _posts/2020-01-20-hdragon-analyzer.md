---
# 포스트 제목을 기재합니다.
title:  "웹 성능 분석 툴 소개" 
# 저자를 입력합니다. 
author: "hdragon"
# 카테고리를 입력합니다.
categories: ["Angular"]
# 태그를 입력합니다.
tags: ["Angular"]
last_modified_at: 2020-01-20T08:06:00-05:00
---

# 웹 성능 분석 툴

필자는 최근 angular로 웹을 개발하면서 겪은 초기 로딩 문제를 통해 알게 된 웹 성능 분석 사이트 및 패키지들이 있다. 이들이 필자가 직면한 문제에 대한 원인 혹은 앞으로 개선 할 수 있는 사항들에 대한 정보를 충분히 내포하고 있기에 이를 angular로 웹 혹은 웹 앱을 만드는 개발자들을 위해 소개하고자 한다.

1. webpack-bundle-analyzer

     처음으로 소개할 것은 번들 사이즈 분석에 사용하는 webpack-bundle-analyzer다.  webpack-bundle-analyzer는 webpack을 통해 번들링되는 모듈을 시각적으로 보여주며 이를 통해 어떤 번들의 사이즈가 큰지 혹은 필요 이상으로 사이즈가 큰지 등의 정보를 파악할 수 있다.

    ### 설치

        npm install webpack-bundle-analyzer

    ### 사용법

     필자는 다음 명령어를 통해 생성된 파일을 바탕으로 분석을 진행했다.

        npm run build -- --prod --stats-json

     build가 완료되고 dist폴더에 build된 파일들이 생성되면 다음 명령어를 통해 분석을 진행한다.

        webpack-bundle-analyzer dist/stats.json

     그럼 다음과 같이 번들의 크기가 시각적으로 표시된 페이지가 열리게 된다. 이를 토대로 번들을 분석하면 된다.

    필자의 경우 moment에 필요 이상으로 locale이 용량을 차지하고 있다. 이는 moment 전체를 import하여 생긴 것으로 moment.js만 import하는 것으로 최적화를 진행할 수 있다.

    ![webpack bundle analyzer result]({{site.baseurl}}/assets/images/hdragon/wba.png)

     필자가 사용한 방법 외에 플러그인을 이용하는 방법도 있다. 시각화 웹을 띄울 host와 port도 지정할 수 있다.

        cosnt BundleAnalyzerPlugin = require('webpack-bundle-analyzer).BundleAnalyzerPlugin;
        
        module.exports = {
        	plugins: [
        		new BundleAnalyzerPlugin({
        			analyzerHost: '',
        			analyzerPort: ,
        		});
        	]
        }

2. Lighthouse

     다음은 Chrome에서 제공하는 lighthouse다. lighthouse는 Chrome에서 [확장프로그램](https://chrome.google.com/webstore/detail/lighthouse/blipmdconlkpinefehnmjammfjpmpbjk?hl=ko)을 통해 사용할 수도 있고 개발자 도구(F12 혹은 window : ctrl + shift + i, mac : cmd + alt + i)의 audit 탭에서 사용할 수 있다. 로컬 환경의 웹의 경우 확장프로그램이 동작 안하는 경우가 있으므로 필자는 개발자 도구의 audit 탭을 활용할 것을 권한다.

    ### 사용법

     사용법은 간단하다. Chrome으로 분석하고자 하는 페이지에 접속한 후 확장프로그램 혹은 개발자 도구 audit 탭에서 분석을 진행하면 된다.

    ![light house]({{site.baseurl}}/assets/images/hdragon/lighthouse.png)

    ![audit]({{site.baseurl}}/assets/images/hdragon/dt-audit.png)

     mobile/desktop의 장치 환경, 분석할 내용, 인터넷과 장치의 성능 제한 등의 설정을 지정할 수도 있다.

     분석을 진행한 경우 다음과 같은 결과를 볼 수 있고 lighthouse에서 제공하는 권장 수정 사항을 통해 웹의 성능을 분석 및 개선할 수 있다. 다음은 google의 desktop 환경 분석 결과이다.

    ![desktop lighthouse result]({{site.baseurl}}/assets/images/hdragon/web-lighthouse.png)

     개발자 도구의 경우 View Trace를 통해 어느 시점에 어떤 작업이 이루어지고 있는지에 대한 분석도 가능하다.

    ![desktop audit result]({{site.baseurl}}/assets/images/hdragon/web-audit.png)

    ![desktop audit view trace]({{site.baseurl}}/assets/images/hdragon/audit-trace.png)

3. PageSpeed Insights

     마지막으로 google developers에서 제공하는 [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/?hl=ko)에 대해 소개하겠다.

     PageSpeed Insights는 lighthouse를 토대로 점수를 매기는 사이트로 확장프로그램이나 개발자 도구 없이 단순히 웹의 Url을 입력하는 것만으로 분석이 가능하다.

    ![pagespeed insights]({{site.baseurl}}/assets/images/hdragon/psi.png)

     다음은 구글을 분석한 것으로 상단에 mobile/desktop의 장치 환경을 선택할 수 있는 탭이 있고 각 환경에 대한 정보가 나열된다. PageSpeed Insights의 경우 lighthouse와 달리 한 번에 두 장치 환경에서의 분석 결과를 볼 수 있다는 장점이 있다. 다만 개발자 도구의 audit과는 달리 인터넷 환경 및 장치 성능의 제한이 없어 다양한 환경에서의 분석은 불가능하다.

    ![pagespeed insights result]({{site.baseurl}}/assets/images/hdragon/psi-result.png)
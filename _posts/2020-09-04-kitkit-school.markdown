---
title : "교육용 게임 Kitkit school"
excerpt : "Kitkit school, Enuma, Xprize"
category :
  - spark
tag :
  - spark
  - kitkit 
  - educational game
use_math : true
author_profile : true
header:
  teaser : /assets/images/category/data.jpg
  overlay_image : /assets/images/category/data.jpg
  overlay_filter: 0.1
---

# Kitkit School에 대해...

## Kitkit School

![/assets/img/algorithm/dfsbfs.jpg](/assets/img/algorithm/kk1.jpg)

"Kitkit School"은 스타트업 기업 "Enuma"에서 개발한 교육용 게임입니다. Enuma는 수학 교육용 게임인 "Todo 수학"이라는 앱으로도 알려져 있는 회사인데요, 이 회사에서 내놓은 "Kitkit School"이라는 게임은 초등학교 저학년 수준의 언어(영어), 수학을 학습할 수 있게 디자인된 게임입니다. 제가 속한 연구실에서는 Enuma와 함께 협업을 하였는데, 바로 이 "Kitkit School"로 수집한 로그 데이터를 처리하고 분석하는 일이었습니다. 

## Xprize

![/assets/img/algorithm/dfsbfs.jpg](/assets/img/algorithm/kk2.jpg)

(c) Enuma

"Kitkit School"이 특별한 이유는, 바로 이 게임이 "XPRIZE"라는 재단이 개최한 컨테스트에서 당당하게 우승을 차지했다는 점입니다. XPRIZE는 비영리 벤처 재단인데, 교육 시설이 부족한 수백명의 아프리카의 어린이들에게 15개월 간 무료로 게임과 태블릿을 제공하고 필드 테스트를 거쳐서 가장 훌륭한 교육용 애플리케이션을 선정하였답니다. 그 결과 "Kitkit School"의 Enuma는 "Onebillion"이라는 단체와 함께 공동 우승을 하게 되었습니다. 두 우승자는 테슬라의 CEO인 일론 머스크가 후원한 상금을 전달받았습니다. 

XPRIZE에서 우승을 차지한 "Kitkit Schhol"은 현재 깃허브에 그 소스 코드가 무료로 공개되어 있습니다. 소스 코드는 [여기](https://github.com/XPRIZE/GLEXP-Team-KitkitSchool)서 찾을 수 있습니다. Cocos2d로 만들었다고 하는데요, 안드로이드 스튜디오가 있으면 직접 빌드해서 apk 파일을 생성할 수 있습니다. 

## 탄자니아 어린이들

![/assets/img/algorithm/dfsbfs.jpg](/assets/img/algorithm/kk3.jpg)

(c) Enuma

필드 테스트는 2016년 ~ 2017년에 탄자니아의 두개의 학교에서 이루어졌습니다. 

## Kitkit School의 데이터 분석

"Kitkit School" 게임은 플레이를 하면서 실시간으로 어떤 플레이를 했는지를 기록한 로그 데이터가 기기의 로컬 경로에 저장되도록 개발되어 있습니다. 앞으로의 포스트에서는 수집된 데이터를 어떻게 가공하는지에 대해 말씀드리겠습니다.
## Introduction
- judge에서 diff이 적용된 데이터를 받아 이를 활용하여 결과 확인 시 __유저의 정답과 실제 정답__ 을 비교하여 색깔을 입혀 나타내줍니다.
- 일치 할 경우: 파란색, 불일치 할 경우: 붉은색의 텍스트 배경을 가집니다.


![image](https://user-images.githubusercontent.com/68943056/144068709-f347bf3b-1fe2-4cee-90dc-2debdccf06d2.png)

## The Applied Part
- templates/submission/status-testcases.html 
- resources/submission.scss

## BackEnd Repository
https://github.com/tuna1210/JOTA-dmoj-judge-server/blob/feature/diff/docs/diff/README_DIFF_VISUALIZER.md

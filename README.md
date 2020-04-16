# EBSAutoComplete
## Script By TriNet / PMH#7086

### 지원 범위
EBS 중학, EBSi

### 스크립트
#### EBS 중학
```js
function getToEBS (lctreSn) {
  // by @pmh-only
  var saveUrl = '/user/lecture/status/save'
  if (IS_SDL) saveUrl = '/user/lecture/status/sdlSave'
  
  const param =
    'courseId=' + courseID +
    '&stepId=' + stepID +
    '&lectureId=' + lctreSn +
    '&enrollNo=' + enrollNO +
    '&encodingTypeCode=' + encType +
    '&lastStudyTime=' + 99999 +
    '&lastStudyLocation=' + 99999 +
    '&partAccumulateStudyTime=' + 0

  console.log(`Sending Success Code to ${lctreSn}`)
  makeRequest(saveUrl, param)
}

const arr = []
$('.titlez').each((_, e) => arr.push(eval(e.onclick.toString().split(',')[2])))
arr.forEach(getToEBS)
```

#### EBSi
```js
function postToEBS (_, lctreSn) {
  const lessonId = lctreSn.id
  const sbjtapplyId = window.frmStudyPlayer.sbjtapplyId.value
  const sbjtId = window.frmStudyPlayer.sbjtId.value

  // 학습 중용 데이터
  let value = { lessonId, sbjtapplyId, sbjtId, clntGbnCd: "C", second: "1", lecGbn: "V500", atndGbnCd: "S", ltStdTm: "1", status: "0" }

  // 학습중으로 변경
  jQuery.ajax({
    type: 'POST',
    async: false,
    url: '/ebs/lms/lmsHist/saveLmsLessonHistDtlAjax.ebs',
    data: value,
    cache: false,
    success: function() {
      console.log('Success: ' + lessonId)
    }
  })

  // 학습 완료용 데이터
  value = { lessonId, sbjtapplyId, eventType: "N" }

  // 학습 완료로 변경
  jQuery.ajax({
    type: 'POST',
    async: false,
    url: '/ebs/lms/lmsHist/saveLmsLessonHistCompletedAjax.ebs',
    data: value,
    cache: false,
    success: function() {
      console.log('Success: ' + lessonId)
    }
  })
}

jQuery('tbody.lessonList>tr').each(postToEBS)
```

### 사용법
1. EBS 강의 보기 페이지에 접속한다
2. EBS 강의 보기 페이지에서 F12 또는 CTRL + SHIFT + I 를 누른다<br />
2-1. 개발자 도구 탭에서 Console 을 클릭한다.<br />
2-2. 콘솔창에 아래의 코드를 복사 붙여넣기 한다 (CTRL C => CTRL V)<br />
2-3. 엔터키를 누른후 오류가 난다면 Issue 에 오류 내용을 첨부해주고, 완료될때 까지 기다린다 (Success 글씨가 더이상 뜨지 않을 때 까지)<br />
2-4. 완료되었다면 새로고침을 한다.<br />

> Copyright (c) PMH (pmhstudio.pmh@gmail.com)

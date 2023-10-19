---
title:  "[유데미X스나이퍼팩토리] 10주 완성 프로젝트 캠프 유니티 - 5주차 - 미니프로젝트"

categories:
  - UdemyXSniperFactory-PreJobTrainning
tags:
  - [프로젝트캠프후기, 유데미, udemy, 스나이퍼팩토리, 웅진씽크빅, 인사이드아웃, IT개발캠프, 개발자부트캠프, unity, 유니티, 게임개발, 메타버스 ]

toc: true
toc_sticky: true
 
date: 2023-10-11
last_modified_at: 2023-10-13
---
<br>

팀으로 그동안 배운 것을 응용하여 미니게임을 만들었다.

## 만든 게임

좀비에게 도망가며 탈출에 필요한 열쇠들을 수집하는 게임, 여러 엔딩이 존재하며, 히든 엔딩을 보기 위해 숨겨진 오브젝트를 수집해야 한다.

## 내가 맡은 역할

UI&UX, FX

## 구현한 내용

### 1\. HUD

UIManager.cs (싱글톤 구현)

```c++
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class UIManager : MonoBehaviour
{
    public static UIManager instance
    {
        get
        {
            if (m_instance == null)
            {
                m_instance = FindObjectOfType<UIManager>();
            }
            return m_instance;
        }
    }

    private static UIManager m_instance;

    // HP바
    [SerializeField] private Slider slider;

    // Key 오브젝트들
    [SerializeField] private GameObject KeyL;
    [SerializeField] private GameObject KeyL_Null;
    [SerializeField] private GameObject KeyR;
    [SerializeField] private GameObject KeyR_Null;
    [SerializeField] private GameObject KeyF;
    [SerializeField] private GameObject KeyF_Null;
    [SerializeField] private GameObject Ring;
    [SerializeField] private GameObject Ring_Null;

    // OpenDoor 팝업
    [SerializeField] private GameObject DoorPopUp;
    // Propose 팝업
    [SerializeField] private GameObject ProposePopUp;

    // 공격 받으면 화면 붉어짐
    [SerializeField] private GameObject AttackedEffectPanel;

    // 게임오버
    [SerializeField] private GameObject GameOverPanel;


    // HP바 설정, Player 스크립트에서 공격 받을 시 호출
    public void UpdateHealthBar(float damage)
    {
        slider.value -= damage;
        if (slider.value <= 0)
            slider.value = 0;
    }

    // Kye 관리, Player 스크립트나 열쇠 스크립트로부터 정보를 받아 호출
    public void UpdateKeys(int value)
    {
        switch (value)
        {
            case 0:
                KeyL_Null.SetActive(false);
                KeyL.SetActive(true);   // 열쇠 이미지 활성화
                break;
            case 1:
                KeyR_Null.SetActive(false);
                KeyR.SetActive(true);
                break;
            case 2:
                KeyF_Null.SetActive(false);
                KeyF.SetActive(true);
                break;
            case 3:
                Ring_Null.SetActive(false);
                Ring.SetActive(true);
                break;
        }
    }

    // 문 클릭 시 팝업 활성화
    public void DoorEndingPopUp()
    {
        DoorPopUp.SetActive(true);
    }
    // 팝업 yes, 엔딩 씬 로드
    public void OpenDoor()
    {
        SceneManager.LoadScene("Ending");
    }
    // 팝업 No, 팝업 지우기
    public void ClosePopUp()
    {
        DoorPopUp.SetActive(false);
    }

    // 반지 들고 문 클릭 시 팝업 활성화
    public void ProposeEndingPopUp()
    {
        ProposePopUp.SetActive(true);
    }
    // 팝업 yes, 히든 엔딩 씬 로드
    public void ProposeYes()
    {
        SceneManager.LoadScene("HiddenEnding");
    }
    // 팝업 No, 팝업 지우기
    public void ProposeNo()
    {
        ProposePopUp.SetActive(false);
    }

    // 공격 받을 시 화면 붉어짐
    public void Attacked()
    {
        StartCoroutine("AttackedEffect");
    }

    // 2초 동안 화면 붉어짐 효과 코루틴
    IEnumerator AttackedEffect()
    {
        AttackedEffectPanel.SetActive(true);
        yield return new WaitForSeconds(0.7f);
        AttackedEffectPanel.SetActive(false);
    }
    
    // GameOver //
    // 게임 오버 패널 활성화
    public void GameOver()
    {
        GameOverPanel.SetActive(true);
    }
    // 재시작 버튼 클릭, 메인 Scene 로드
    public void LoadMain()
    {
        SceneManager.LoadScene("Main");
    }

    // 나가기 버튼 클릭, 시작 Scene 로드
    public void Exit()
    {
        SceneManager.LoadScene("Start");
    }
}
```
<br>
**기본 화면**

![HUD1](https://github.com/JeongSooHyeon/Algorithm/assets/82567002/871fd58d-f95c-4f7f-a095-6e75d805e2ed){: width="100%", height="100%"}



**HP바**
- UpdateHealthBar() 함수
    - Player가 공격 받으면 slider.value 값 감소
    - Player 스크립트에서 호출

**아이템 창**

![HUD3](https://github.com/JeongSooHyeon/Algorithm/assets/82567002/173b45b6-64fe-409e-be78-1fc159d907fe){: width="100%", height="100%"}
- UpdateKey() 함수
    - 열쇠, 반지를 먹으면 숫자에 따라 left, right, finish 각자리에 이미지 활성화
    - Item 스크립트에서 호출

**문 클릭 시 팝업**
-	PopUp() : 팝업창 활성화
-	OpenDoor() : yes 버튼 클릭 시, 엔딩 씬 로드
-	ClosePopUp() : no 버튼 클릭 시, 팝업창 비활성화

**반지를 획득했을 때 팝업**

![RingPopUp](https://github.com/JeongSooHyeon/Algorithm/assets/82567002/7e661028-5968-46b3-9cc2-2201c7f0dd65){: width="100%", height="100%"}

- ProposeEndingPopUp() : 팝업창 활성화
- ProposeYes() : yes 버튼 클릭 시, 히든엔딩 화면 로드
- ProposeNo() : No 버튼 클릭 시, 팝업창 비활성화

**공격 받았을 때 화면 효과**

![Attackted](https://github.com/JeongSooHyeon/Algorithm/assets/82567002/e2cd302a-3561-4336-83fa-027bdd60cef1){: width="100%", height="100%"}
-	Attacked() : “AttackedEffect” 코루틴 호출
    - Player스크립트에서 호출
-	AttackedEffect() 코루틴 : 2초동안 붉은 화면 활성화 후 비활성화

**게임 오버**

![GameOver](https://github.com/JeongSooHyeon/Algorithm/assets/82567002/8b316ea7-143f-4c43-b65d-70b997bdb880){: width="100%", height="100%"}
-	GameOver() : 게임 오버 패널 활성화
-	LoadMain() : 재시작 버튼 클릭 시 Main 씬 로드
-	Exit() : 나가기 버튼 클릭 시 Start 씬 로드

### 2\. 게임 시작 UI
 
GameStart.cs 

```c++
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class GameStart : MonoBehaviour
{
    // 팝업창 관리
    [SerializeField] private GameObject CreditPopUp;
    [SerializeField] private GameObject LicencePopUp;
    [SerializeField] private Text txt;
    [SerializeField] private Text Title;

    // 기본 시작 화면
    [SerializeField] private GameObject ButtonPanel;

    // 나가기 버튼 눌렀을 때 팝업
    [SerializeField] private GameObject ExitPopUp;

    // 게임 시작 클릭 시, Main 씬 로드
    public void LoadMain()
    {
        SceneManager.LoadScene("Main");
    }

    // 크레딧 클릭 시, 팝업 활성화
    public void Credit()
    {
        ButtonPanel.SetActive(false);   
        CreditPopUp.SetActive(true);  
    }

    // 라이선스 클릭 시, 팝업 활성화
    public void Licence()
    {
        ButtonPanel.SetActive(false);   
        LicencePopUp.SetActive(true);  
    }

    // 나가기 클릭 시, 팝업 활성화
    public void ClickExit()
    {
        ExitPopUp.SetActive(true);

    }
    // 게임 종료
    public void ExitGame()
    {
        Application.Quit(); // 빌드 안 하면 실행 XXX
    }
    // 진짜 나갈 건지 되묻는 팝업창
    public void CloseExitPopUp()
    {
        ButtonPanel.SetActive(true);   
        ExitPopUp.SetActive(false);  
    }

    // 아니오 버튼 클릭 시, 팝업창 닫기
    public void CloseCreditPopUp()
    {
        ButtonPanel.SetActive(true);   
        CreditPopUp.SetActive(false);  
    }

 // 아니오 버튼 클릭 시, 팝업창 닫기
    public void CloseLecencePopUp()
    {
        ButtonPanel.SetActive(true);   
        LicencePopUp.SetActive(false);   
    }
}
```

**게임 시작 화면**

![GameStart](https://github.com/JeongSooHyeon/Algorithm/assets/82567002/f471aa39-6be0-4a49-85b8-32863dc4c611){: width="100%", height="100%"}

**게임 시작 버튼 클릭** : Main 씬 로드

**크레딧, 라이선스 버튼 클릭** 

![Credit](https://github.com/JeongSooHyeon/Algorithm/assets/82567002/c1a43037-5088-4dd9-a1bb-2f6008297426){: width="50%", height="50%"}![Licence](https://github.com/JeongSooHyeon/Algorithm/assets/82567002/0bd6acd8-95f9-4dbe-8132-86cd45342cfc){: width="50%", height="50%"}

- 팝업창 활성화, 뒤로가기 버튼 눌러서 끄기

**게임 종료 하기**
![GameExit](https://github.com/JeongSooHyeon/Algorithm/assets/82567002/3a944545-2f77-47a0-a718-a948f665da98){: width="100%", height="100%"}
-	진짜 종료할 건지 되묻는 팝업창 활성화
-	예 선택 시, Application.Quit()으로 게임 나가기
-	아니오 선택 시, 팝업창 끄기


### 3\. 게임 엔딩 UI

GameClear.cs

```c++
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class GameClear : MonoBehaviour
{
    // 처음으로 버튼 클릭, 시작 Scene 로드
    public void LoadStart()
    {
        SceneManager.LoadScene("Start");
    }
}
```

**게임 엔딩 화면**

![Ending](https://github.com/JeongSooHyeon/Algorithm/assets/82567002/0a1edd1f-4194-4269-9897-8ef3c7d90b37){: width="100%", height="100%"}

**히든 엔딩 화면**

![HiddenEnding](https://github.com/JeongSooHyeon/Algorithm/assets/82567002/cb8fdda1-4502-4f66-9e6a-ed46e54b1e97){: width="100%", height="100%"}

- 처음으로 버튼 클릭 시, Start 씬 로드

### 4\. Item 이펙트

Item(열쇠, 반지, 액자)에 Light, Particle System 컴포넌트를 추가하여 아이템 강조 효과 적용

**Items의 계층 구조**

![Item_Effect](https://github.com/JeongSooHyeon/Algorithm/assets/82567002/9ec548f7-2eff-48eb-8449-4478049fdaf2){: width="30%", height="30%"}

- 아이템에 빈 오브젝트를 추가하여 Light와 Particle System을 각각 넣었다 

**Light와 Particle**

![Item_Light](https://github.com/JeongSooHyeon/Algorithm/assets/82567002/8778dbb5-fc72-404c-aaf0-c2ae28ed1619){: width="50%", height="50%"}![Item_Particle](https://github.com/JeongSooHyeon/Algorithm/assets/82567002/9fc282de-34a6-47f9-8724-1a2783a071e4){: width="50%", height="50%"}

**이펙트가 적용된 모습**

![ItemEffect](https://github.com/JeongSooHyeon/Algorithm/assets/82567002/8778b332-bb92-47c8-8db8-0b2f7d6d4d61){: width="100%", height="100%"}

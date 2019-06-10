## Compute > Image > 콘솔 사용 가이드

## 이미지 생성

이미지는 인스턴스의 기본 디스크에서 만들 수 있습니다. u2 타입의 인스턴스를 제외한 t2, m2, c2, r2, x1 타입의 인스턴스는 실행 중에 이미지 생성을 지원하지만 데이터 정합성을 보장하지 않습니다. U 타입의 인스턴스는 종료 상태의 인스턴스만 이미지 생성이 가능합니다.

Windows 인스턴스의 이미지를 생성하려면 Sysprep을 이용하여 이미지 생성을 준비한 다음 인스턴스를 종료하는 것을 권장합니다. 자세한 sysprep 사용 방법은 [Windows Sysprep 가이드](#windows-sysprep)를 참고합니다.

실행 중인 Windows 인스턴스를 이미지로 생성할 경우 2019.05.28 배포 버전 이전의 이미지로 만든 Windows 인스턴스라면 정확한 동작을 위해 선행 작업이 필요합니다. 인스턴스를 생성한 이미지의 Windows 버전은 **인스턴스 상세정보**의 **이미지 이름**을 통해 확인할 수 있습니다. 자세한 내용은 [실행 중인 Windows 인스턴스 이미지 생성 가이드](#windows)를 참고합니다.

## 이미지 수정

이미지의 이름을 수정합니다.

**Protected** 속성을 선택하고 이미지를 업데이트하면 실수로 이미지를 삭제하는 것을 방지할 수 있습니다. Protected 속성이 선택된 이미지를 삭제하려면, 이미지 수정에서 **Protected** 속성을 선택 해제하고 이미지를 업데이트해야 합니다.

## 다른 프로젝트에 공유

이미지를 공유할 프로젝트를 선택하고 공유합니다.


## Windows Sysprep 가이드

Windows 이미지를 생성하려면 하드웨어와 사용자에 종속된 정보를 제거하여 인스턴스 생성에 사용할 수 있도록 이미지 초기화 작업이 필요합니다. 이미지 초기화는 Microsoft에서 Windows OS를 배포하기 위해 제공하는 시스템 준비 유틸리티인 Sysprep에서 실행할 수 있습니다.

먼저 Windows 인스턴스에 접속한 후 **앱**에서 **명령 프롬프트**를 마우스 오른쪽 버튼으로 클릭하고 **관리자 권한으로 실행**을 클릭합니다.
![[그림 1 명령 프롬프트 실행]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/001_170524_800px.PNG)

명령 프롬프트 창이 뜨면, 아래 명령을 실행합니다.

	cd C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf
	C:\Windows\System32\Sysprep\sysprep.exe /generalize /oobe /shutdown /unattend:Unattend.xml

![[그림 2 Sysprep 실행]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/002_170524_800px.PNG)

Sysprep이 실행되면 Windows 인스턴스는 자동으로 종료됩니다. TOAST 콘솔에서 Windows 인스턴스의 종료를 확인하고, [이미지 생성](./console-guide/#_1) 기능으로 사용자 Windows 이미지를 생성합니다.

### Sysprep 옵션 상세 정보


`\generalize`

Windows에 등록된 고유 시스템 정보를 제거합니다. 이 단계에서 SID(Security ID)가 다시 설정되고, 시스템 복원 지점이 모두 지워지며, 이벤트 로그가 삭제됩니다. 이 단계를 거친 후 재부팅을 하면 `Windows 구성 단계`가 나타납니다.
> [참고]
이 단계에서 SID 및 사용자 정보와 고유 정보를 삭제하여, 기존 프로그램의 동작에 영향을 줄 수 있습니다.


`\oobe`
Windows를 재부팅하여 시작 모드로 진입한 후 사용자가 미리 지정한 설정을 적용합니다. 이 때 적용할 수 있는 설정으로는 대표적으로 국가별 설정, 네트워크 위치, 언어 설정, 시간대 등이 있습니다.

`\shutdown`
Windows를 종료합니다.

`\unattend`
Windows를 재설치한 뒤, 앞 단계에서 기록한 사용자 설정을 복구합니다. 이 단계에서는 Windows 사용자 정보를 등록하고, 드라이버 및 제품 업데이트 및 추가 소프트웨어를 설치합니다. 또한 기본으로 제공하는 설정 외에 사용자가 원하는 설정을 응답 파일에서 지정할 수 있습니다.

> [참고]
TOAST에서 제공하는 Windows 이미지의 응답 파일은 C:\Program Files\Cloud Solutions\Cloudbase-Init\conf\Unattend.xml에 있습니다. 필요한 설정은 모두 준비되었기 때문에 특별한 용도를 제외하면 수정하지 않아도 됩니다.


## 실행 중인 Windows 인스턴스 이미지 생성 가이드

실행 중인 Windows 인스턴스로 이미지를 생성할 때 원본 인스턴스의 이미지 버전이 2019.05.28 이전이라면 아래의 선행 작업이 필요합니다.
만약 2019.05.28 배포 버전 이후의 Windows 이미지라면 아래의 과정은 불필요합니다.

Windows 인스턴스에 접속한 후 **앱**에서 Task Scheduler를 실행합니다.
![[그림 3 Task Scheudler 실행]](http://static.toastoven.net/prod_infrastructure/compute/windows/001_190604.png)

Task Scheduler 우측 Actions 탭의 **Create Task...**를 클릭합니다.
![[그림 4 Create Task 시작]](http://static.toastoven.net/prod_infrastructure/compute/windows/002_190604.png)

Create Task 팝업의 **General 탭**에서 Name에 적절한 이름을 넣고 **Security options**의 **Run with highest privileges**를 체크한 뒤 **Change User or Group...**를 클릭합니다.
![[그림 5 User 변경 및 우선순위 설정]](http://static.toastoven.net/prod_infrastructure/compute/windows/003_190604.png)

Enter the object name to select 하단의 텍스트 박스에 **SYSTEM**을 입력 후 OK를 클릭합니다.
![[그림 6 SYSTEM 유저 설정]](http://static.toastoven.net/prod_infrastructure/compute/windows/004_190604.png)

Create Task 팝업의 **Triggers** 탭으로 이동하여 New 버튼을 클릭하여 새로운 Trigger를 생성합니다.
![[그림 7 Trigger 설정 1]](http://static.toastoven.net/prod_infrastructure/compute/windows/005_190604.png)

Begin the task의 트리거를 **At startup**으로 선택 후 OK를 클릭하여 Trigger 생성을 완료합니다.
![[그림 8 Trigger 설정 2]](http://static.toastoven.net/prod_infrastructure/compute/windows/006_190604.png)

Create Task 팝업의 **Actions 탭**으로 이동하여 New 버튼을 클릭하여 새로운 Action을 생성합니다.
![[그림 9 Action 설정 1]](http://static.toastoven.net/prod_infrastructure/compute/windows/007_190604.png)

**Action 생성 팝업**에서 아래의 값을 입력합니다.

	Program/script: C:\Windows\System32\ipconfig.exe
	Add arguments(optional): /renew

![[그림 10 Action 설정 2]](http://static.toastoven.net/prod_infrastructure/compute/windows/008_190604.png)

OK를 클릭하여 New Action 팝업 및 Create Task 팝업을 닫으며 스케줄러 등록을 완료합니다.

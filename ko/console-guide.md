## Compute > Image > 콘솔 사용 가이드

## 이미지 생성

이미지는 인스턴스의 루트 블록 스토리지에서 만들 수 있습니다. u2 타입의 인스턴스를 제외한 t2, m2, c2, r2, x1 타입의 인스턴스에서는 실행 중에도 이미지를 생성할 수 있지만, 데이터 정합성은 보장하지 않습니다. u2 타입 인스턴스에서는 중지 상태일 때만 이미지를 생성할 수 있습니다.

Linux 인스턴스의 이미지를 생성하기 전 machine-id를 초기화하여 중복을 예방하는 것을 권장합니다. 자세한 machine-id 초기화 방법은 [Linux machine-id 초기화 가이드](#Linux-machineid)를 참고합니다.

Windows 인스턴스의 이미지를 생성하려면 Sysprep을 이용하여 이미지 생성을 준비한 다음 인스턴스를 중지하는 것을 권장합니다. 자세한 sysprep 사용 방법은 [Windows Sysprep 가이드](#windows-sysprep)를 참고합니다.

실행 중인 Windows 인스턴스를 이미지로 생성할 경우, 2019. 05.28. 배포 버전 이전의 이미지로 만든 Windows 인스턴스라면 정확한 동작을 위해 선행 작업이 필요합니다. 인스턴스를 생성한 이미지의 Windows 버전은 **인스턴스 상세정보**의 **이미지 이름**에서 확인할 수 있습니다. 자세한 내용은 [실행 중인 Windows 인스턴스 이미지 생성 가이드](#windows)를 참고합니다.

## 이미지 수정

이미지의 이름을 수정합니다.

**Protected** 속성을 선택하고 이미지를 업데이트하면 실수로 이미지를 삭제하는 것을 방지할 수 있습니다. Protected 속성이 선택된 이미지를 삭제하려면, 이미지 수정에서 **Protected** 속성을 선택 해제하고 이미지를 업데이트해야 합니다.

## 다른 리전으로 복제

이미지를 복제할 대상 리전을 선택하고, 새 이미지의 이름과 설명을 입력한 후 복제합니다.

## 다른 프로젝트에 공유

이미지를 공유할 프로젝트를 선택하고 공유합니다.

## Linux machine-id 초기화 가이드

Linux 사용자 이미지를 만들어 해당 이미지로 인스턴스를 생성할 경우 machine-id가 중복되어 예기치 못한 문제가 발생할 수 있습니다.
사용자 이미지 생성 전 machine-id를 초기화하여 중복을 예방할 수 있습니다.

	$ sudo sh -c "echo -n > /etc/machine-id"
	$ sudo rm /var/lib/dbus/machine-id
	$ sudo ln -s /etc/machine-id /var/lib/dbus/machine-id

## Windows Sysprep 가이드

Windows 이미지를 생성하려면 하드웨어와 사용자에 종속된 정보를 제거하여 인스턴스 생성에 사용할 수 있도록 이미지 초기화 작업이 필요합니다. 이미지 초기화는 Microsoft에서 Windows OS를 배포하기 위해 제공하는 시스템 준비 유틸리티인 Sysprep에서 실행할 수 있습니다.

### 원본 인스턴스의 이미지 버전이 2020. 08. 18. 이후인 경우
Windows 인스턴스에 접속한 후 **PowerShell**을 관리자 권한으로 실행합니다.
![[그림 1 Powershell 실행]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/win_sysprep1.png)

PowerShell 창이 나타나면 아래 명령을 실행합니다.

    ToastSysprep

![[그림 2 ToastSysprep 실행]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/win_sysprep2.png)
> [참고]
ToastSysprep은 NHN Cloud에서 제공하는 Sysprep을 간편하게 사용할 수 있는 명령어입니다.

**Y** 키를 눌러 작업을 진행합니다.
![[그림 3 ToastSysprep 진행]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/win_sysprep3.png)

Sysprep이 실행되면 Windows 인스턴스는 자동으로 중지됩니다. NHN Cloud 콘솔에서 Windows 인스턴스의 중지를 확인하고, [이미지 생성](./console-guide/#_1) 기능으로 사용자 Windows 이미지를 생성합니다.

Sysprep을 이용하여 Windows 인스턴스를 초기화하면 비밀번호가 공백으로 변경되어 로그인할 수 없습니다. 이미지 생성 기능을 이용할 때, **이미지로 생성되는 Windows 비밀번호를 초기화합니다.** 옵션을 선택해 Windows 인스턴스의 비밀번호를 자동으로 초기화하는 것이 좋습니다. 초기화된 비밀번호는 인스턴스 접속 정보에서 확인합니다.

### 원본 인스턴스의 이미지 버전이 2020. 08. 18. 이전인 경우

먼저 Windows 인스턴스에 접속한 후 **앱**에서 **명령 프롬프트**를 마우스 오른쪽 버튼으로 클릭하고 **관리자 권한으로 실행**을 클릭합니다.
![[그림 1 명령 프롬프트 실행]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/001_170524_800px.PNG)

명령 프롬프트 창이 뜨면, 아래 명령을 실행합니다.

	sc config cloudbase-init start= demand
	cd C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf
	C:\Windows\System32\Sysprep\sysprep.exe /generalize /oobe /shutdown /unattend:Unattend.xml

![[그림 2 Sysprep 실행]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/002_170524_800px.PNG)

Sysprep이 실행되면 Windows 인스턴스는 자동으로 중지됩니다. NHN Cloud 콘솔에서 Windows 인스턴스의 중지를 확인하고, [이미지 생성](./console-guide/#_1) 기능으로 사용자 Windows 이미지를 생성합니다.

Sysprep을 이용하여 Windows 인스턴스를 초기화하면 비밀번호가 공백으로 변경되어 로그인할 수 없습니다. 이미지 생성 기능을 이용할 때, **이미지로 생성되는 Windows 비밀번호를 초기화합니다.** 옵션을 선택해 Windows 인스턴스의 비밀번호를 자동으로 초기화하는 것이 좋습니다. 초기화된 비밀번호는 인스턴스 접속 정보에서 확인합니다.

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
NHN Cloud에서 제공하는 Windows 이미지의 응답 파일은 C:\Program Files\Cloud Solutions\Cloudbase-Init\conf\Unattend.xml에 있습니다. 필요한 설정은 모두 준비되었기 때문에 특별한 용도를 제외하면 수정하지 않아도 됩니다.


## 실행 중인 Windows 인스턴스 이미지 생성 가이드

실행 중인 Windows 인스턴스로 이미지를 생성할 때 원본 인스턴스의 이미지 버전이 2019. 05. 28. 이전이라면 아래의 선행 작업이 필요합니다.
만약 2019. 05. 28. 배포 버전 이후의 Windows 이미지라면 아래의 과정은 불필요합니다.

1. Windows 인스턴스에 접속한 후 **앱**에서 **Task Scheduler**를 실행합니다.
![[그림 3 Task Scheudler 실행]](http://static.toastoven.net/prod_infrastructure/compute/windows/001_190604.png)

2. **Task Scheduler** 오른쪽 **Actions** 탭의 **Create Task**를 클릭합니다.
![[그림 4 Create Task 시작]](http://static.toastoven.net/prod_infrastructure/compute/windows/002_190604.png)

3. **Create Task** 창의 **General** 탭에서 **Name**에 이름을 입력하고 **Security options**의 **Run with highest privileges**를 선택한 뒤 **Change User or Group**을 클릭합니다.
![[그림 5 User 변경 및 우선순위 설정]](http://static.toastoven.net/prod_infrastructure/compute/windows/003_190604.png)

4. **Enter the object name to select** 하단의 입력 상자에 **SYSTEM**을 입력하고 **OK** 버튼을 클릭합니다.
![[그림 6 SYSTEM 유저 설정]](http://static.toastoven.net/prod_infrastructure/compute/windows/004_190604.png)

5. **Create Task** 창의 **Triggers** 탭에서 **New** 버튼을 클릭하여 새로운 트리거(trigger)를 생성합니다.
![[그림 7 Trigger 설정 1]](http://static.toastoven.net/prod_infrastructure/compute/windows/005_190604.png)

6. **Begin the task**의 트리거를 **At startup**으로 선택하고 **OK** 버튼을 클릭하여 트리거 생성을 완료합니다.
![[그림 8 Trigger 설정 2]](http://static.toastoven.net/prod_infrastructure/compute/windows/006_190604.png)

7. **Create Task** 창의 **Actions** 탭에서 **New** 버튼을 클릭하여 새로운 액션(action)을 생성합니다.
![[그림 9 Action 설정 1]](http://static.toastoven.net/prod_infrastructure/compute/windows/007_190604.png)

8. **Action 생성** 창에서 아래의 값을 입력합니다.

```
	Program/script: C:\Windows\System32\ipconfig.exe
	Add arguments(optional): /renew
```

![[그림 10 Action 설정 2]](http://static.toastoven.net/prod_infrastructure/compute/windows/008_190604.png)

9. **OK** 버튼을 클릭하여 **New Action** 창과 **Create Task** 창을 닫고 스케줄러 등록을 완료합니다.

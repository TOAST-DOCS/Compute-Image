## Compute > Image > 콘솔 사용 가이드

## 이미지 생성

이미지는 인스턴스의 기본 디스크에서 만들 수 있습니다. 데이터 정합성을 보장하기 위해 인스턴스를 종료한 상태에서만 이미지를 생성할 수 있습니다. 종료 상태의 인스턴스가 없으면 이미지를 생성할 수 없습니다.

Windows 인스턴스의 이미지를 생성하려면 Sysprep을 이용하여 이미지 생성을 준비한 다음 인스턴스를 종료해야 합니다. 자세한 sysprep 사용 방법은 [Windows Sysprep 가이드](#windows-sysprep)를 참고합니다.

Linux 인스턴스 이미지를 생성하려면 인스턴스 내에서 종료하거나 TOAST 콘솔에서 종료하여 이미지를 생성합니다.

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

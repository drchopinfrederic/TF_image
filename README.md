# Toss Front 1.5 원본 이미지

토스 프론트 1.5(Rockchip RK3399) 한 대에서 2026-09-08에 확보한 원본 이미지와
검증 기록입니다. 같은 전체 원본을 해당 기기에 다시 쓴 뒤 전체 재읽기 해시 일치와
정상 부팅·화면·터치 반응을 확인했습니다.

## 다운로드

이미지 파일은 [Releases](https://github.com/drchopinfrederic/Toss_Front_1.5_Image/releases/tag/backup-2026-09-08)의
**Assets**에서 받습니다. GitHub의 `Code → Download ZIP`에는 안내와 검증 기록이 들어 있습니다.

| 파일 | 내용 |
| --- | --- |
| `toss-front15-20260908-emmc-user.zip` | 원본 전체 `emmc-user.img`와 내부 검증 파일 |
| `toss-front15-20260908-emmc-user.zip.sha256` | 전체 덤프 ZIP의 SHA-256 |
| `toss-front15-20260908-emmc-user-verification.json` | 전체 덤프의 압축 전후 해시·크기·ZIP CRC 검증 결과 |
| `toss-front15-20260908-partitions-no-userdata.zip` | `userdata`를 제외한 전체 13개 파티션의 개별 `.img` 및 내부 검증 파일 |
| `toss-front15-20260908-partitions-no-userdata.zip.sha256` | 파티션 ZIP의 SHA-256 |
| `toss-front15-20260908-partitions-no-userdata-verification.json` | 13개 이미지의 압축 전후 해시·크기·ZIP CRC 검증 결과 |

### ZIP 내부 구조

**전체 복원용 ZIP** — eMMC 사용자 영역 전체가 `emmc-user.img` 한 파일에 들어 있습니다.

```text
toss-front15-20260908-emmc-user.zip
├── emmc-user.img     15,636,365,312바이트 — 전체 원본 이미지
├── README.md         백업 범위·검증·복원 안내
├── manifest.json     이미지 크기·SHA-256·백업 범위
└── SHA256SUMS        emmc-user.img의 SHA-256
```

**개별 파티션 ZIP (`userdata` 제외)** — 원본 GPT 파티션 14개 중 `userdata`만 제외한
13개를 각각 `.img` 파일로 담았습니다.

```text
toss-front15-20260908-partitions-no-userdata.zip
├── security.img           4,194,304바이트 (4MiB)
├── uboot.img              4,194,304바이트 (4MiB)
├── trust.img              4,194,304바이트 (4MiB)
├── misc.img               4,194,304바이트 (4MiB)
├── dtbo.img               4,194,304바이트 (4MiB)
├── vbmeta.img             1,048,576바이트 (1MiB)
├── boot.img              41,943,040바이트 (40MiB)
├── recovery.img         125,829,120바이트 (120MiB)
├── backup.img           402,653,184바이트 (384MiB)
├── cache.img            402,653,184바이트 (384MiB)
├── metadata.img          16,777,216바이트 (16MiB)
├── baseparameter.img      1,048,576바이트 (1MiB)
├── super.img          2,147,483,648바이트 (2GiB)
├── README.md          파티션 구성·검증 안내
├── manifest.json      각 파티션의 크기·시작 LBA·SHA-256
└── SHA256SUMS         13개 이미지 각각의 SHA-256
```

위 트리는 실제 ZIP 내부 파일 목록입니다. 다운로드 목록의 `.zip.sha256`과
`-verification.json`은 **ZIP 밖에서 따로 받는 파일**이며, 내부의 `SHA256SUMS`와
`manifest.json`은 압축을 풀면 나옵니다.

이 파티션 세트는 13개의 원본 raw 이미지로 구성됩니다. `super.img`도 raw 형식이며
Android sparse 이미지로 변환하지 않았습니다. **제외한 GPT 파티션은 `userdata` 하나**이며,
`security`, `cache`, `metadata`를 포함한 나머지 파티션은 원본 그대로입니다.
GPT 파티션 표와 파티션 바깥의 초기 로더 영역은 이 세트에 포함하지 않습니다.
이 영역까지 포함한 원본 기기 전체 복원에는 `emmc-user.img`를 사용합니다.
이 개별 파티션 세트만을 기록하는 복원 시험은 수행하지 않았습니다.

**전체 덤프 ZIP에는 `userdata`·`security`를 포함한 기기 원본이 그대로 들어 있습니다.**
사용 데이터나 기기 설정을 삭제·익명화한 펌웨어가 아닙니다. 원래 백업을 수집한
기기의 전체 복원에는 `emmc-user.img`를 사용합니다. 압축 해제 시 약 15.6GB의
저장 공간이 추가로 필요합니다.

ZIP을 사용한 이유는 GitHub Releases의 개별 첨부 파일이 2GiB 미만이어야 하기
때문입니다. 전체 원본은 약 15.6GB이며, `super.img`도 정확히 2GiB입니다.
압축을 풀면 원본과 동일한 바이트의 이미지가 나옵니다.
[GitHub의 파일 제한](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases)

## 파티션 정보

모든 주소는 이 원본 기기에서 확인한 값이며 섹터 크기는 512바이트입니다.

| 이미지 | 크기(바이트) | 시작 LBA | 섹터 수 |
| --- | ---: | ---: | ---: |
| `security.img` | 4,194,304 | 8192 | 8192 |
| `uboot.img` | 4,194,304 | 16384 | 8192 |
| `trust.img` | 4,194,304 | 24576 | 8192 |
| `misc.img` | 4,194,304 | 32768 | 8192 |
| `dtbo.img` | 4,194,304 | 40960 | 8192 |
| `vbmeta.img` | 1,048,576 | 49152 | 2048 |
| `boot.img` | 41,943,040 | 51200 | 81920 |
| `recovery.img` | 125,829,120 | 133120 | 245760 |
| `backup.img` | 402,653,184 | 378880 | 786432 |
| `cache.img` | 402,653,184 | 1165312 | 786432 |
| `metadata.img` | 16,777,216 | 1951744 | 32768 |
| `baseparameter.img` | 1,048,576 | 1984512 | 2048 |
| `super.img` | 2,147,483,648 | 1986560 | 4194304 |

`super.img`의 크기는 **4,194,304섹터 × 512바이트 = 2,147,483,648바이트**입니다.
정확히 **2GiB**이며, 십진수 GB로는 약 **2.15GB**입니다. 시작 LBA `1986560`은
기기에서 이 파티션이 시작하는 섹터 번호입니다.

## 다운로드 검증

Windows PowerShell에서 ZIP의 해시를 구하고 `.sha256` 파일의 값과 비교합니다.

```powershell
Get-FileHash -Algorithm SHA256 .\toss-front15-20260908-emmc-user.zip
Get-Content .\toss-front15-20260908-emmc-user.zip.sha256
Get-FileHash -Algorithm SHA256 .\toss-front15-20260908-partitions-no-userdata.zip
Get-Content .\toss-front15-20260908-partitions-no-userdata.zip.sha256
```

ZIP을 푼 뒤 각 이미지의 해시를 내부 `SHA256SUMS`와 비교할 수도 있습니다.

```powershell
Get-FileHash -Algorithm SHA256 .\*.img
Get-Content .\SHA256SUMS
```

Linux/WSL에서는 ZIP과 이미지 디렉터리에서 각각 다음 명령을 사용합니다.

```sh
sha256sum -c toss-front15-20260908-emmc-user.zip.sha256
sha256sum -c toss-front15-20260908-partitions-no-userdata.zip.sha256
# 압축을 푼 디렉터리에서 실행
sha256sum -c SHA256SUMS
```

## 원본 검증과 복원 시험

공통 원본은 eMMC 사용자 영역 LBA 0부터 끝까지 읽은 `emmc-user.img`입니다.
크기는 **15,636,365,312바이트**, GPT 파티션은 14개입니다. 초기 로더 영역,
`uboot`·`trust`, 안드로이드 및 사용자 데이터를 포함하며, 별도 eMMC **boot0·boot1·RPMB는
백업 범위에 포함하지 않습니다.**

원본 전체 이미지 SHA-256:

```text
4a005193c762c411e244e3068470036764057fae424d84d4e26d72ee82993c22
```

- 앞뒤 GPT 헤더·항목 배열의 CRC와 원본 기기 파티션 표 일치 확인.
- 별도 USB 읽기로 11개 구간을 다시 수집해 원본과 바이트 단위로 대조.
- 추출한 14개 파티션의 해시와 이미지 헤더 확인.
- 같은 기기에 전체 원본을 복원하고, 재부팅 전에 약 15.6GB 전체를 다시 읽어
  크기와 SHA-256 일치 확인.
- 재부팅 후 소유자가 평소 화면과 터치 반응의 정상 동작 확인.

[원본 검증 요약](verification/source-backup.json)과
[전체 덤프 배포본 검증 결과](verification/full-backup.json),
[파티션 배포본 검증 결과](verification/partitions.json)에 기계 판독 기록이 있습니다.
두 ZIP은 별도 폴더에 압축을 풀어 각각의 `SHA256SUMS`를 확인합니다.

이 결과는 원본을 수집한 동일 기기에서의 시험입니다. 다른 단말기나 VAN사별 호환성,
결제 기능은 검증하지 않았습니다. 다른 기기에 기록할 때는 해당 기기의 파티션 구성과
원본 백업을 먼저 확인해야 합니다.

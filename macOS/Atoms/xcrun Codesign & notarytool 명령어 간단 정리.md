
> 공식문서 : https://developer.apple.com/documentation/security/notarizing_macos_software_before_distribution/customizing_the_notarization_workflow

- Codesign
```bash
codesign --force --sign "Developer ID Application: ~~~" <codesign 할 파일 경로>
```

- Codesign with `.entitlement`
```bash
codesign --force --deep --sign "Developer ID Application: ~~~" \ 
	--entitlements <직접 생성한 .entitlement 파일 경로> <codesign 할 파일 경로>
```

- 키체인에 2FA 암호 등록
```bash
xcrun notarytool store-credentials "앱 암호(2FA)의 이름" \
               --apple-id "Apple 계정 (이메일)" \
               --team-id My_Team_ID \  // ex) ASDF1E2F34
               --password 2FA_PASSWORD  // ex) asdf-asdf-asdf-asdf
```

- 공증 시도
```bash
xcrun notarytool submit <공증할 파일(dmg 등)의 경로> \
                   --keychain-profile "앱 암호(2FA)의 이름" \
                   --wait \
                   --webhook "https://example.com/notarization"
```

- 공증 로그 확인하기
```bash
xcrun notarytool log <UUID> \
	 --keychain-profile "앱 암호(2FA)의 이름" developer_log.json
```

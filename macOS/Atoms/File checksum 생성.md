> https://m.blog.naver.com/kiros33/130157574926


## SHA256
```bash
$ shasum -a 256 filename.ext
7ab2bc6554759bcc3dd2a8494418c79c0cfafc7b8fad2c8d8f57495bb0a6e974 filename.ext
$ openssl dgst -sha256 filename.ext 
SHA256(filename.ext)= 7ab2bc6554759bcc3dd2a8494418c79c0cfafc7b8fad2c8d8f57495bb0a6e974
```

## MD5
```bash
$ shasum -a 256 filename.ext
7ab2bc6554759bcc3dd2a8494418c79c0cfafc7b8fad2c8d8f57495bb0a6e974  filename.ext

$ openssl dgst -sha256 filename.ext
SHA256(filename.ext)= 7ab2bc6554759bcc3dd2a8494418c79c0cfafc7b8fad2c8d8f57495bb0a6e974
```

## Others
```bash
$ cksum filename.ext
479433993 4620011 filename.ext

$ sum filename.ext
63231 4512 filename.ext
```
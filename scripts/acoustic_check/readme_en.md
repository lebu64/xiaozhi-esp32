# Acoustic Wave Test
This GUI is used to test receiving PCM data from Xiaozhi devices via `udp` and converting it to time domain/frequency domain. It can save sound of window length, used to determine noise frequency distribution and test the accuracy of acoustic wave transmission ASCII.

Firmware testing requires enabling `USE_AUDIO_DEBUGGER` and setting `AUDIO_DEBUG_UDP_SERVER` to the local machine address.
Acoustic wave `demod` can be output through `sonic_wifi_config.html` or by uploading to PinMe's [Xiaozhi Acoustic Wave Network Configuration](https://iqf7jnhi.pinit.eth.limo) for acoustic wave testing.

# Acoustic Wave Decoding Test Records

> `✓` represents successful decoding when receiving raw PCM signal through I2S DIN, `△` represents stable decoding possible with noise reduction or additional operations, `X` represents poor effect even after noise reduction (may decode partially but very unstable).
> Some ADCs require more precise noise reduction adjustments during I2C configuration phase. Due to device non-universality, only testing according to config provided in boards.

| Device | ADC | MIC | Effect | Notes |
| ---- | ---- | --- | --- | ---- |
| bread-compact | INMP441 | Integrated MEMEMIC | ✓ |
| atk-dnesp32s3-box | ES8311 | | ✓ |
| magiclick-2p5 | ES8311 | | ✓ |
| lichuang-dev  | ES7210 | | △ | Need to turn off INPUT_REFERENCE during testing
| kevin-box-2 | ES7210 | | △ | Need to turn off INPUT_REFERENCE during testing
| m5stack-core-s3 | ES7210 | | △ | Need to turn off INPUT_REFERENCE during testing
| xmini-c3 | ES8311 | | △ | Requires noise reduction
| atoms3r-echo-base | ES8311 | | △ | Requires noise reduction
| atk-dnesp32s3-box0 | ES8311 | | X | Can receive and decode, but high packet loss rate
| movecall-moji-esp32s3 | ES8311 | | X | Can receive and decode, but high packet loss rate

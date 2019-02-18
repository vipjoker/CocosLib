
set(COCOS_AUDIO_SRC
    audio/AudioEngine.cpp
    )

if(ANDROID)
    set(COCOS_AUDIO_PLATFORM_SRC
        audio/android/AudioEngine-inl.cpp
        audio/android/ccdandroidUtils.cpp
        audio/android/cddSimpleAudioEngine.cpp
        audio/android/AudioPlayerProvider.cpp
        audio/android/UrlAudioPlayer.cpp
        audio/android/PcmData.cpp
        audio/android/CCThreadPool.cpp
        audio/android/AudioMixerController.cpp
        audio/android/PcmAudioService.cpp
        audio/android/PcmAudioPlayer.cpp
        audio/android/PcmBufferProvider.cpp
        audio/android/AudioDecoderProvider.cpp
        audio/android/AudioDecoder.cpp
        audio/android/AudioDecoderOgg.cpp
        audio/android/AudioDecoderMp3.cpp
        audio/android/AudioDecoderWav.cpp
        audio/android/AudioDecoderSLES.cpp
        audio/android/AudioResampler.cpp
        audio/android/AudioResamplerCubic.cpp
        audio/android/AssetFd.cpp
        audio/android/AudioMixer.cpp
        audio/android/Track.cpp
        audio/android/mp3reader.cpp
        audio/android/tinysndfile.cpp
        audio/android/audio_utils/format.c
        audio/android/audio_utils/minifloat.cpp
        audio/android/audio_utils/primitives.c
        audio/android/utils/Utils.cpp
        audio/android/jni/cddandroidAndroidJavaEngine.cpp
    )

endif()

list(APPEND COCOS_AUDIO_SRC ${COCOS_AUDIO_PLATFORM_SRC})

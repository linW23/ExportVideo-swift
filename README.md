# ExportVideo-swift
自定义导出视频

设置startTime 和 durationTime 可以自定义裁剪视频时长, 如果不设置则默认导出完整视频
设置videoOutputSetting 来设置导出视频的参数
            videoOutputSetting = {
                  let compressionSettings: [String: Any] = [
                        AVVideoAverageNonDroppableFrameRateKey: frameRate, //平均包含不可丢弃的帧率数量
                        AVVideoAverageBitRateKey: bitrate, //视频比特率
                        AVVideoMaxKeyFrameIntervalKey: 30, //帧率
                        AVVideoProfileLevelKey: AVVideoProfileLevelH264HighAutoLevel //编码格式
                    ]
                    
                    var videoSettings: [String : Any] = [
                        AVVideoWidthKey: 540, //分辨率压缩到 540 * 960 
                        AVVideoHeightKey: 960,
                        AVVideoCompressionPropertiesKey: compressionSettings
                    ]
                    if #available(iOS 11.0, *) {
                        videoSettings[AVVideoCodecKey] =  AVVideoCodecType.h264
                    } else {
                        videoSettings[AVVideoCodecKey] =  AVVideoCodecH264
                    }
                    return videoSettings
            }()
                    
设置audioOutputSetting 来设置导出时中音频参数
                  audioOutputSetting = {
                    var stereoChannelLayout = AudioChannelLayout()
                    memset(&stereoChannelLayout, 0, MemoryLayout<AudioChannelLayout>.size)
                    stereoChannelLayout.mChannelLayoutTag = kAudioChannelLayoutTag_Stereo
                    
                    let channelLayoutAsData = Data(bytes: &stereoChannelLayout, count: MemoryLayout<AudioChannelLayout>.size)
                    let compressionAudioSettings: [String: Any] = [
                        AVFormatIDKey: kAudioFormatMPEG4AAC,
                        AVEncoderBitRateKey: 128000,
                        AVSampleRateKey: 44100, //采样率
                        AVChannelLayoutKey: channelLayoutAsData,
                        AVNumberOfChannelsKey: 2
                    ]
                    return compressionAudioSettings
                  }()
                  

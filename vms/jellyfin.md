# jellyfin server
VM Network IP: 172.16.1.7/24

Install Debian. Install jellyfin. https://jellyfin.org/downloads/linux

```
# sudo nano /etc/systemd/journald.conf
Storage=none

# cat /etc/fstab
//172.16.0.3/media  /mnt/nas_media  cifs  rw,username=user,password=XXXX,nofail  0 0
sand_jellyfin	/sand/scratch/jellyfin	virtiofs	rw,noatime,_netdev,nofail

sudo systemctl daemon-reload
sudo systemctl enable systemd-networkd-wait-online.service
```

```
<?xml version="1.0" encoding="utf-8"?>
<EncodingOptions xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <EncodingThreadCount>-1</EncodingThreadCount>
  <TranscodingTempPath>/tmp/jellyfin/transcodes</TranscodingTempPath>
  <FallbackFontPath />
  <EnableFallbackFont>false</EnableFallbackFont>
  <EnableAudioVbr>false</EnableAudioVbr>
  <DownMixAudioBoost>2</DownMixAudioBoost>
  <DownMixStereoAlgorithm>None</DownMixStereoAlgorithm>
  <MaxMuxingQueueSize>2048</MaxMuxingQueueSize>
  <EnableThrottling>true</EnableThrottling>
  <ThrottleDelaySeconds>180</ThrottleDelaySeconds>
  <EnableSegmentDeletion>false</EnableSegmentDeletion>
  <SegmentKeepSeconds>720</SegmentKeepSeconds>
  <HardwareAccelerationType>qsv</HardwareAccelerationType>
  <EncoderAppPathDisplay>/usr/lib/jellyfin-ffmpeg/ffmpeg</EncoderAppPathDisplay>
  <VaapiDevice>/dev/dri/renderD128</VaapiDevice>
  <QsvDevice />
  <EnableTonemapping>false</EnableTonemapping>
  <EnableVppTonemapping>true</EnableVppTonemapping>
  <EnableVideoToolboxTonemapping>false</EnableVideoToolboxTonemapping>
  <TonemappingAlgorithm>bt2390</TonemappingAlgorithm>
  <TonemappingMode>auto</TonemappingMode>
  <TonemappingRange>auto</TonemappingRange>
  <TonemappingDesat>0</TonemappingDesat>
  <TonemappingPeak>100</TonemappingPeak>
  <TonemappingParam>0</TonemappingParam>
  <VppTonemappingBrightness>16</VppTonemappingBrightness>
  <VppTonemappingContrast>1</VppTonemappingContrast>
  <H264Crf>23</H264Crf>
  <H265Crf>28</H265Crf>
  <EncoderPreset>auto</EncoderPreset>
  <DeinterlaceDoubleRate>false</DeinterlaceDoubleRate>
  <DeinterlaceMethod>yadif</DeinterlaceMethod>
  <EnableDecodingColorDepth10Hevc>true</EnableDecodingColorDepth10Hevc>
  <EnableDecodingColorDepth10Vp9>true</EnableDecodingColorDepth10Vp9>
  <EnableDecodingColorDepth10HevcRext>false</EnableDecodingColorDepth10HevcRext>
  <EnableDecodingColorDepth12HevcRext>false</EnableDecodingColorDepth12HevcRext>
  <EnableEnhancedNvdecDecoder>true</EnableEnhancedNvdecDecoder>
  <PreferSystemNativeHwDecoder>true</PreferSystemNativeHwDecoder>
  <EnableIntelLowPowerH264HwEncoder>false</EnableIntelLowPowerH264HwEncoder>
  <EnableIntelLowPowerHevcHwEncoder>false</EnableIntelLowPowerHevcHwEncoder>
  <EnableHardwareEncoding>false</EnableHardwareEncoding>
  <AllowHevcEncoding>false</AllowHevcEncoding>
  <AllowAv1Encoding>false</AllowAv1Encoding>
  <EnableSubtitleExtraction>true</EnableSubtitleExtraction>
  <HardwareDecodingCodecs>
    <string>h264</string>
    <string>hevc</string>
    <string>vc1</string>
  </HardwareDecodingCodecs>
  <AllowOnDemandMetadataBasedKeyframeExtractionForExtensions>
    <string>mkv</string>
  </AllowOnDemandMetadataBasedKeyframeExtractionForExtensions>
</EncodingOptions>
```

---
title: "Widevine 라이선스 템플릿 개요 | Microsoft Docs"
description: "이 토픽에서는 Widevine 라이선스를 구성하는 데 사용되는 Widevine 라이선스 템플릿의 개요를 제공합니다."
author: juliako
manager: erikre
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0e6f1f05-7ed6-4ed6-82a0-0cc2182b075a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: a90e56bb2b7db0bb964684f9cac04096a6577adc


---
# <a name="widevine-license-template-overview"></a>Widevine 라이선스 템플릿 개요
## <a name="overview"></a>개요
이제 Azure 미디어 서비스를 사용하면 Widevine 라이선스를 요청하고 구성할 수 있습니다. 최종 사용자 플레이어가 Widevine으로 보호된 콘텐츠를 재생하려고 하면 라이선스 배달 서비스로 요청을 보내 라이선스를 얻습니다. 라이선스 서비스에서 요청을 승인하면 클라이언트로 전송하여 지정된 콘텐츠의 암호를 해독하고 재생하는 데 사용할 수 있는 라이선스가 발급됩니다.

Widevine 라이선스 요청 형식은 JSON 메시지입니다.  

"{}"로 값이 없는 빈 메시지를 만들도록 선택할 수 있으며 모든 기본값이 포함된 라이선스 템플릿이 만들어집니다.  

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

## <a name="json-message"></a>JSON 메시지
| 이름 | 값 | 설명 |
| --- | --- | --- |
| payload |Base64 인코딩된 문자열 |클라이언트에서 보낸 라이선스 요청입니다. |
| content_id |Base64 인코딩된 문자열 |각 content_key_specs.track_type에 대한 KeyId 및 콘텐츠 키를 파생시키는 데 사용되는 ID입니다. |
| provider |string |콘텐츠 키 및 정책을 조회하는 데 사용합니다. 필수입니다. |
| policy_name |string |이전에 등록된 정책의 이름입니다. 옵션 |
| allowed_track_types |enum |SD_ONLY 또는 SD_HD. 라이선스에 포함할 콘텐츠 키를 제어합니다. |
| content_key_specs |JSON 구조 배열. 아래 **콘텐츠 키 사양** 참조 |반환할 콘텐츠 키에 대한 보다 세분화된 제어. 자세한 내용은 아래 콘텐츠 키 사양을 참조하세요.  Allowed_track_types 및 content_key_specs 중 하나만 지정할 수 있습니다. |
| use_policy_overrides_exclusively |boolean. true 또는 false |policy_overrides로 지정된 정책 특성을 사용하고 이전에 저장된 모든 정책은 생략합니다. |
| policy_overrides |JSON 구조. 아래 **정책 재정의** 참조 |이 라이선스에 대한 정책 설정입니다.  이 자산에 미리 정의된 정책이 있는 경우 이러한 지정된 값이 사용됩니다. |
| session_init |JSON 구조. 아래 **세션 초기화** 참조 |라이선스에 전달되는 선택적 데이터입니다. |
| parse_only |boolean. true 또는 false |라이선스 요청이 구문 분석되지만 라이선스는 발급되지 않습니다. 그러나 라이선스 요청을 구성하는 값은 응답에 반환됩니다. |

## <a name="content-key-specs"></a>콘텐츠 키 사양
기존 정책이 있는 경우 콘텐츠 키 사양에 값을 지정할 필요가 없습니다.  이 콘텐츠와 연결된 기존 정책이 HDCP 및 CGMS와 같은 출력 보호를 확인하는 데 사용됩니다.  기존 정책이 Widevine 라이선스 서버에 등록되지 않은 경우 콘텐츠 공급자는 값을 라이선스 요청에 삽입할 수 있습니다.   

use_policy_overrides_exclusively 옵션에 관계없이 모든 트랙에 대해 각 content_key_specs를 지정해야 합니다. 

| 이름 | 값 | 설명 |
| --- | --- | --- |
| content_key_specs. track_type |string |트랙 유형 이름입니다. 라이선스 요청에 Content_key_specs를 지정한 경우 모든 트랙 유형을 명시적으로 지정해야 합니다. 이렇게 하지 않으면 이전 10초 재생에 실패합니다. |
| content_key_specs  <br/> security_level |uint32 |재생에 대한 클라이언트 견고성 요구 사항을 정의합니다. <br/>  1 - 소프트웨어 기반 화이트 박스 암호화가 필요합니다. <br/>  2 - 소프트웨어 암호화 및 난독 처리된 디코더가 필요합니다. <br/>  3 - 하드웨어 기반의 신뢰할 수 있는 실행 환경에서 키 자료 및 암호화 작업을 수행해야 합니다. <br/>  4 - 하드웨어 기반의 신뢰할 수 있는 실행 환경에서 콘텐츠의 암호화 및 디코딩을 수행해야 합니다.  <br/>  5 - 하드웨어 기반의 신뢰할 수 있는 실행 환경에서 미디어의 암호화, 디코딩 및 모든 처리(압축 및 압축 해제)를 수행해야 합니다. |
| content_key_specs <br/> required_output_protection.hdc |string - HDCP_NONE, HDCP_V1, HDCP_V2 중 하나 |HDCP가 필요한지 여부를 나타냅니다. |
| content_key_specs <br/>key |Base64  <br/>인코딩된 문자열 |이 트랙에 사용할 콘텐츠 키입니다. 지정된 경우 track_type 또는 key_id가 필요합니다.  이 옵션을 사용하면 Widevine 라이선스 서버에서 키를 생성하거나 조회하도록 하지 않고, 콘텐츠 공급자가 이 트랙에 대한 콘텐츠 키를 삽입할 수 있습니다. |
| content_key_specs.key_id |Base64 인코딩된 문자열 binary, 16바이트 |키의 고유 식별자. |

## <a name="policy-overrides"></a>정책 재정의
| 이름 | 값 | 설명 |
| --- | --- | --- |
| policy_overrides. can_play |boolean. true 또는 false |콘텐츠의 재생이 허용됨을 나타냅니다. 기본값은 false입니다. |
| policy_overrides. can_persist |boolean. true 또는 false |라이선스를 오프라인 용도로 비휘발성 저장소에 유지할 수 있음을 나타냅니다. 기본값은 false입니다. |
| policy_overrides. can_renew |boolean. true 또는 false |이 라이선스의 갱신이 허용됨을 나타냅니다. true이면 라이선스의 기간을 하트비트로 확장할 수 있습니다. 기본값은 false입니다. |
| policy_overrides. license_duration_seconds |int64 |이 특정 라이선스에 대한 기간을 나타냅니다. 값 0은 기간에 제한이 없음을 나타냅니다. 기본값은 0입니다. |
| policy_overrides. rental_duration_seconds |int64 |재생이 허용되는 동안 기간을 나타냅니다. 값 0은 기간에 제한이 없음을 나타냅니다. 기본값은 0입니다. |
| policy_overrides. playback_duration_seconds |int64 |라이선스 기간 내에서 재생이 시작된 후 표시 기간입니다. 값 0은 기간에 제한이 없음을 나타냅니다. 기본값은 0입니다. |
| policy_overrides. renewal_server_url |string |이 라이선스에 대한 모든 하트비트(갱신) 요청은 지정된 URL로 전달됩니다. 이 필드는 can_renew가 true인 경우에만 사용할 수 있습니다. |
| policy_overrides. renewal_delay_seconds |int64 |license_start_time 이후, 처음으로 갱신을 시도할 때까지 시간(초)입니다. 이 필드는 can_renew가 true인 경우에만 사용할 수 있습니다. 기본값은 0입니다. |
| policy_overrides. renewal_retry_interval_seconds |int64 |실패 시 후속 라이선스 갱신 요청 간 지연 시간(초)을 지정합니다. 이 필드는 can_renew가 true인 경우에만 사용할 수 있습니다. |
| policy_overrides. renewal_recovery_duration_seconds |int64 |갱신을 시도하는 동안 재생을 진행하도록 허용되었으나 라이선스 서버와 백 엔드 문제로 인해 재생이 실패한 기간입니다. 값 0은 기간에 제한이 없음을 나타냅니다. 이 필드는 can_renew가 true인 경우에만 사용할 수 있습니다. |
| policy_overrides. renew_with_usage |boolean. true 또는 false |사용이 시작된 경우 갱신을 위해 라이선스가 전송되어야 함을 나타냅니다. 이 필드는 can_renew가 true인 경우에만 사용할 수 있습니다. |

## <a name="session-initialization"></a>세션 초기화
| 이름 | 값 | 설명 |
| --- | --- | --- |
| provider_session_token |Base64 인코딩된 문자열 |이 세션 토큰은 라이선스에 다시 전달되고 후속 갱신에 존재합니다.  세션 토큰은 세션 이후에는 유지되지 않습니다. |
| provider_client_token |Base64 인코딩된 문자열 |라이선스 응답으로 다시 보낼 클라이언트 토큰입니다.  클라이언트 요청에 클라이언트 토큰이 포함된 경우 이 값은 무시됩니다. 클라이언트 토큰은 라이선스 세션 이후에 유지됩니다. |
| override_provider_client_token |boolean. true 또는 false |false이고 라이선스 요청에 클라이언트 토큰이 포함된 경우 이 구조에 클라이언트 토큰이 지정된 경우에도 요청의 토큰을 사용합니다.  true이면 항상 이 구조에 지정된 토큰을 사용합니다. |

## <a name="configure-your-widevine-licenses-using-net-types"></a>.NET 형식을 사용하여 Widevine 라이선스 구성
미디어 서비스는 Widevine 라이선스를 구성할 수 있는 .NET API를 제공합니다. 

### <a name="classes-as-defined-in-the-media-services-net-sdk"></a>미디어 서비스 .NET SDK에 정의된 클래스
이러한 형식에 대한 정의는 다음과 같습니다.

    public class WidevineMessage
    {
        public WidevineMessage();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

### <a name="example"></a>예
다음 예제는 .NET API를 사용하여 간단한 Widevine 라이선스를 구성하는 방법을 보여줍니다.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


## <a name="media-services-learning-paths"></a>미디어 서비스 학습 경로
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>피드백 제공
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>참고 항목
[PlayReady 및/또는 Widevine 동적 일반 암호화 사용](media-services-protect-with-drm.md)




<!--HONumber=Nov16_HO3-->



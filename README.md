# Freqtrade Kripto Para Ticaret Botu

Bu proje, Freqtrade kullanarak kripto para piyasalarında otomatik ticaret yapmak için geliştirilmiş özel bir strateji içermektedir.

## 📋 Proje Hakkında

Bu proje, **kendi geliştirdiğim özel ticaret stratejisi** ile kripto para piyasalarında otomatik işlem yapmak için tasarlanmıştır. Strateji, teknik analiz göstergelerini kullanarak alım-satım sinyalleri üretmektedir.

## 🎯 Özellikler

- **Özel Strateji**: `combineStrategy` adında kendi geliştirdiğim strateji
- **Futures Trading**: Binance futures piyasasında işlem yapma desteği
- **Docker Desteği**: Kolay kurulum ve çalıştırma için Docker container desteği
- **Telegram Bildirimleri**: İşlem bildirimleri için Telegram entegrasyonu
- **REST API**: Bot kontrolü için REST API desteği
- **Hyperopt Optimizasyonu**: Strateji parametrelerini optimize etme desteği

## 🔧 Strateji Detayları

### combineStrategy

Bu strateji, aşağıdaki teknik analiz göstergelerini kullanarak alım-satım sinyalleri üretir:

#### Kullanılan Göstergeler:
- **TEMA (Triple Exponential Moving Average)**: 9 periyotluk
- **Bollinger Bands**: 20 periyot, 2 standart sapma
- **Heikin Ashi**: Mum grafiği analizi
- **RSI (Relative Strength Index)**: Momentum göstergesi (Hyperopt ile optimize edilebilir)
- **MACD**: Trend takip göstergesi
- **ADX**: Trend gücü göstergesi
- **Parabolic SAR**: Trend dönüş sinyalleri
- **Stochastic Fast**: Momentum osilatörü
- **MFI (Money Flow Index)**: Para akışı göstergesi

#### Alım Koşulları:
- TEMA, Bollinger Bands orta bandının altında
- TEMA yükseliş trendinde
- Heikin Ashi yeşil mum (yükseliş)

#### Satım Koşulları:
- TEMA, Bollinger Bands orta bandının üstünde
- TEMA düşüş trendinde
- Heikin Ashi kırmızı mum (düşüş)

### Strateji Parametreleri:
- **Timeframe**: 15 dakika
- **Minimal ROI**: %1 (60 dakika içinde)
- **Stoploss**: %3
- **Max Open Trades**: 10
- **Startup Candle Count**: 200

## 📁 Proje Yapısı

```
Freqtradee-main/
├── ft_userdata/
│   ├── docker-compose.yml          # Docker yapılandırması
│   └── user_data/
│       ├── config.json              # Freqtrade ana yapılandırma dosyası
│       ├── strategies/
│       │   └── combine_strategy.py # Kendi geliştirdiğim özel strateji
│       ├── hyperopts/
│       │   └── sample_hyperopt_loss.py  # Hyperopt loss fonksiyonu
│       ├── notebooks/
│       │   └── strategy_analysis_example.ipynb  # Strateji analiz notebook'u
│       ├── logs/                    # Log dosyaları
│       └── data/                    # Veri dosyaları
└── README.md                        # Bu dosya
```

## 🚀 Kurulum ve Kullanım

### Gereksinimler
- Docker ve Docker Compose
- Binance API anahtarları (opsiyonel - dry-run modu için gerekli değil)

### Kurulum Adımları

1. **Projeyi klonlayın veya indirin**

2. **Docker Compose ile çalıştırın:**
```bash
cd ft_userdata
docker compose up -d
```

3. **Bot durumunu kontrol edin:**
```bash
docker compose logs -f freqtrade
```

### Yapılandırma

Ana yapılandırma dosyası `ft_userdata/user_data/config.json` içinde bulunmaktadır.

#### Önemli Ayarlar:
- **Dry Run**: Şu anda `true` olarak ayarlanmış (test modu)
- **Trading Mode**: `futures` (vadeli işlemler)
- **Margin Mode**: `isolated` (izole marj)
- **Exchange**: Binance
- **Pairlist**: VolumePairList - En yüksek hacimli 20 coin
- **Telegram**: Bildirimler için yapılandırılmış
- **API Server**: Port 8080'de aktif

#### API Anahtarları (Opsiyonel)
Gerçek işlem yapmak için `config.json` dosyasında Binance API anahtarlarınızı eklemeniz gerekir:
```json
"exchange": {
    "name": "binance",
    "key": "YOUR_API_KEY",
    "secret": "YOUR_SECRET_KEY"
}
```

## 📊 Strateji Optimizasyonu

Strateji parametreleri Hyperopt ile optimize edilebilir. RSI parametreleri için optimize edilebilir aralıklar tanımlanmıştır:

- **buy_rsi**: 1-50 arası (varsayılan: 30)
- **sell_rsi**: 50-100 arası (varsayılan: 70)
- **short_rsi**: 51-100 arası (varsayılan: 70)
- **exit_short_rsi**: 1-50 arası (varsayılan: 30)

## 🔍 API Kullanımı

REST API, `http://localhost:8080` adresinde çalışmaktadır.

Varsayılan kullanıcı bilgileri:
- **Username**: freqtrader
- **Password**: Mert.urper73

## ⚠️ Önemli Notlar

1. **Dry Run Modu**: Şu anda bot test modunda çalışmaktadır. Gerçek işlem yapmak için `config.json` içinde `"dry_run": false` yapın ve API anahtarlarınızı ekleyin.

2. **Risk Yönetimi**: Bu strateji eğitim ve test amaçlıdır. Gerçek para ile kullanmadan önce kapsamlı testler yapın.

3. **API Güvenliği**: API anahtarlarınızı asla paylaşmayın ve güvenli bir şekilde saklayın.

## 📝 Loglar

Log dosyaları `ft_userdata/user_data/logs/` dizininde saklanmaktadır. Bot çalışırken logları takip etmek için:

```bash
docker compose logs -f freqtrade
```

## 🤝 Katkıda Bulunma

Bu proje kişisel bir ticaret stratejisi içermektedir. Kendi stratejinizi geliştirmek için `combine_strategy.py` dosyasını inceleyebilir ve özelleştirebilirsiniz.

## 📄 Lisans

Bu proje kişisel kullanım içindir.

## 👤 Geliştirici

Bu strateji ve yapılandırma tarafımdan geliştirilmiştir. Sorularınız için iletişime geçebilirsiniz.

---

**Uyarı**: Kripto para ticareti yüksek risk içerir. Yatırım yapmadan önce kendi araştırmanızı yapın ve sadece kaybetmeyi göze alabileceğiniz parayla işlem yapın.


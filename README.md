# xyolgezerx

> Bu dosya yalnızca OmniRoute içindir. academic-research-skills eklentisi ayrıdır: `docs/academic-research-skills.md`.

## OmniRoute kurulumu

[OmniRoute](https://github.com/cheaper-inference/OmniRoute), yapay zekâ isteklerini tek bir adresten birçok sağlayıcıya yönlendiren, kendi başına çalışan bir sunucudur (npm paketi: `omniroute`, MIT lisanslı). Bu adımlar kendi bilgisayarınız içindir ve OmniRoute 3.8.54 README'sine dayanır.

### 1. Gereksinim

Node.js **22.22.2 veya üstü 22.x** ya da **24–26** sürümü gerekir. Sürümünüzü `node -v` ile kontrol edin.

### 2. Kurup başlatın

**npm ile:**

```bash
npm install -g omniroute
omniroute
```

npm 11 veya üstü kurulum betiklerini engellerse, uyarıda adı geçen paketlere izin verin. README'de verilen liste şu:

```bash
npm install -g --allow-scripts=omniroute,keytar,onnxruntime-node,@parcel/watcher,@swc/core,protobufjs,esbuild omniroute
```

**ya da Docker ile:**

```bash
docker run -d --name omniroute --restart unless-stopped --stop-timeout 40 \
  -p 127.0.0.1:20128:20128 -v omniroute-data:/app/data diegosouzapw/omniroute:latest
```

Claude Code gibi bir kodlama aracını bunun üzerinden kullanacaksanız daha fazla bellek verin: `-e OMNIROUTE_MEMORY_MB=8192 --memory=10g`. Varsayılan 1 GB, uzun bağlamlarda programın çökmesine yol açar.

### 3. Sağlayıcı ekleyin

1. Panel `http://localhost:20128`, API `http://localhost:20128/v1` adresinde açılır.
2. Panelde **Providers** bölümüne girip kendi API anahtarınızı ya da hesabınızı ekleyin.
3. Kendinize ait, yine panelden bir OmniRoute API anahtarı oluşturun.

### 4. Claude Code'u OmniRoute'a bağlayın (isteğe bağlı)

`~/.claude/settings.json` dosyasındaki `"env"` bölümüne şunları ekleyin:

```json
"env": {
  "ANTHROPIC_BASE_URL": "http://localhost:20128",
  "ANTHROPIC_AUTH_TOKEN": "sk-omniroute-anahtarınız"
}
```

- Adresin sonuna `/v1` eklemeyin.
- OmniRoute belgelerindeki `cat > ~/.claude/settings.json` komutu dosyayı baştan yazar ve mevcut ayarlarınızı siler; satırları elle ekleyin.
- Test etmek için: `claude "say hello"`

### Dikkat

- Bu ayar, Claude Code isteklerinizin tamamını OmniRoute üzerinden geçirir.
- Kullandığınız her sağlayıcının kullanım koşullarına bakın. OmniRoute koşulları riskli bulduğu sağlayıcıları `tos: avoid` olarak işaretler ve otomatik yönlendirmeye almaz.
- Projenin isteğe bağlı bir trafik şifresini çözme (MITM) özelliği vardır. Ne yaptığını bilmiyorsanız açmayın.

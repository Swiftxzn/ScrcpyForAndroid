<!-- markdownlint-disable MD033 -->

# Scrcpy para Android

<a href="https://github.com/Genymobile/scrcpy/blob/master/app/data/icon.svg" title="Modified from the original version">
  <img src="app/src/main/assets/icon/icon.svg" width="128" height="128" alt="scrcpy" align="right" />
</a>

[scrcpy](https://github.com/Genymobile/scrcpy), cliente Android.

Espelhe a tela e o áudio de dispositivos Android conectados por meio do
[ADB sem fio](https://developer.android.com/tools/adb#connect-to-a-device-over-wi-fi), permitindo controlar o dispositivo usando a tela sensível ao toque, o teclado e o mouse.

Não é necessário ter acesso root nem instalar um aplicativo no dispositivo.

> [!NOTE]
> Este projeto é baseado no scrcpy, mas não é uma versão oficial. Não há vínculo nem parceria com o autor original ou com a equipe de manutenção.

## Download

\> [releases](https://github.com/Swiftxzn/ScrcpyForAndroid/releases)

## Capturas de tela

<p align="center">
  <img src="https://i.postimg.cc/L6tvNBG3/Screenshot-20261014-031631-Scrcpy-for-Android.jpg" height="300" alt="Dispositivos" />
  <img src="https://i.postimg.cc/WbGSX6x7/Screenshot-20261014-031644-Scrcpy-for-Android.jpg" height="300" alt="Lista" />
  <img src="https://i.postimg.cc/qMsQjXWL/Screenshot-20261014-031656-Scrcpy-for-Android.jpg" height="300" alt="Linguagem" />
  
## Recursos

- Permite abrir o teclado do próprio dispositivo durante o controle, incluindo suporte à digitação em chinês.
- Sincronização da área de transferência.
- Transmissão de áudio com baixa latência (desativada por padrão).
  - Ao reproduzir áudio do tipo `USAGE_MEDIA` no dispositivo controlado ([namidaco/namida](https://github.com/namidaco/namida)), a diferença de latência entre os dois dispositivos é de aproximadamente meio tempo musical (não foi possível medir com precisão).
  - Ao reproduzir áudio do tipo `USAGE_GAME` no dispositivo controlado (Arknights, versão Bilibili), ainda há uma latência perceptível de 100 a 200 ms.
- Preenchimento automático da senha da tela de bloqueio com autenticação biométrica (acessível pelos botões virtuais).
- Alternância entre vários perfis, perfis vinculados a dispositivos e entrada direta no modo de tela cheia após a conexão.
- Permite substituir o `scrcpy-server`.
- Usa o serviço mDNS para conectar automaticamente a dispositivos com depuração sem fio ativada e detectar o IP e a porta de dispositivos aguardando pareamento.
- Pareamento por código QR.
- Alternância automática entre os modos retrato e paisagem.
- Layout em modo paisagem.
  - Disponível apenas em dispositivos cuja proporção de tela seja inferior a 16:9.
- Mapeia a tecla Voltar para o dispositivo remoto no modo de tela cheia.
- Modo picture-in-picture (PiP).
- Transferência de arquivos nos dois sentidos.
- Terminal ADB integrado com transmissão em tempo real.
- Gravação integrada.

## Problemas conhecidos

- No início da transmissão, pode ocorrer perda de quadros-chave, causando artefatos na imagem. Aguarde um pouco até que novos quadros-chave sejam recebidos.
- Como não há dispositivos disponíveis para testes, o aplicativo pode não funcionar corretamente em versões antigas do Android. O recurso picture-in-picture (PiP), em especial, depende bastante da implementação da ROM do fabricante.
- Fechar o picture-in-picture (PiP) não interrompe a transmissão do scrcpy. É necessário voltar ao aplicativo e tocar em Parar.
- Digitação de texto chinês entre dispositivos.
  - A função usa a sincronização da área de transferência, então o histórico de texto digitado acaba sendo adicionado ao histórico da área de transferência do dispositivo controlado.
  - Por motivos ainda desconhecidos, às vezes o texto não aparece na tela.
- A captura de tela pelo botão virtual funciona enviando o código
  `keycode 120`, definido oficialmente pelo Android ([keycodes.h#349](https://android.googlesource.com/platform/frameworks/native/+/master/include/android/keycodes.h#349)) como
  `System Request / Print Screen key`. A implementação varia conforme o fabricante; em algumas ROMs próximas do Android puro, como `AxionOS`, isso pode provocar uma reinicialização suave.
- Após mudar o idioma, é necessário reiniciar o aplicativo para entrar no modo de tela cheia.

## A fazer

\> [TODO.md](TODO.md)

## Fora dos planos

- Compatibilidade com versões antigas do Android.
  - O `minSdk` do projeto é 26 (Android 8), e 98,4% dos dispositivos conseguem instalar o aplicativo. ~~Isso não significa que os problemas serão ignorados.~~
  Se encontrar problemas, especialmente travamentos ou fechamentos inesperados, envie os logs do `logcat` para que seja possível tentar corrigi-los.
- Mais operações com arquivos, incluindo `excluir`, `renomear` e outras.
  - Como alternativa, pressione e segure para copiar o caminho e use `rm` ou `mv` no terminal.
- Melhorias adicionais na gravação.
  - As limitações do `MediaCodec` são grandes; melhorias adicionais exigiriam a inclusão do `ffmpeg`, o que está fora dos planos.
- Instalação de aplicativos via ADB (`adb install`).
  - Isso exigiria mudanças extensas na implementação JNI, então não será feito.
  - Como alternativa, envie o arquivo e instale pelo terminal ou faça a instalação manualmente.

## Registro de alterações

\> [CHANGELOG.md](CHANGELOG.md)

## Módulos recomendados

- Para permitir a captura de telas de bloqueio protegidas por senha: [LSPosed/DisableFlagSecure](https://github.com/LSPosed/DisableFlagSecure)
- Para ativar o ADB automaticamente ao iniciar o dispositivo: [gist/906291](https://gist.github.com/Swiftxzn/9062915f1615d5eebd363c759fda496c)

## Perguntas frequentes

0. Não consigo controlar o dispositivo.
   - [Genymobile/scrcpy/FAQ.md#control-issues](https://github.com/Genymobile/scrcpy/blob/master/FAQ.md#control-issues)

1. O ADB desconecta quando o aplicativo fica em segundo plano.
   - Nas ROMs de alguns fabricantes, altere a configuração de economia de bateria para `Sem restrições`.
   - Nas configurações do Android, ative `Permitir uso em segundo plano` e defina como `Sem restrições` (há um atalho na página de configurações do aplicativo).

2. O teclado não aparece na tela virtual ou aparece na tela principal.
   - Defina `--display-ime-policy` como `local`.
   - Abra o teclado do próprio dispositivo pelo botão flutuante.

3. A taxa de bits só chega a 40 Mbps, e isso é pouco.
   - Toque no título de cada controle deslizante para inserir um valor manualmente.

4. Onde ficam os arquivos gravados ou baixados?
   - /sdcard/Movies/Scrcpy/
   - /sdcard/Download/Scrcpy/

5. O modo paisagem não é muito confortável para canhotos.
   - Há um botão no canto superior direito para inverter a orientação.

6. A tela virtual da MIUI fica sem área de trabalho ou aparece toda branca.
   - Instale um launcher de terceiros.

## Compilação

- JDK 21
- Android SDK (`compileSdk 37` / `buildTools 37.0.0`)
- Android NDK `30.0.16248370` (deve ser compatível com `libcxx`)

```bash
git clone --recursive https://github.com/Swiftxzn/ScrcpyForAndroid.git
cd ScrcpyForAndroid
./gradlew assembleDebug
```

Se o repositório já foi clonado, mas os submódulos não foram baixados:

```bash
git submodule update --init --recursive
```

ABI específica:

```bash
./gradlew assembleRelease -PabiList=arm64-v8a
```

## Agradecimentos

- [Genymobile/scrcpy](https://github.com/Genymobile/scrcpy) (incluindo o ícone)
- Implementação JNI do ADB: [rikkaapps/shizuku](https://github.com/rikkaapps/shizuku), [vvb2060/ndk.boringssl](https://github.com/vvb2060), [lsposed/libcxx](https://github.com/lsposed/libcxx)
- Componentes da interface: [YuKongA/miuix](https://github.com/compose-miuix-ui/miuix)
- Referências de design da interface: [tiann/KernelSU/manager](https://github.com/tiann/KernelSU/tree/main/manager), [miuix/example](https://github.com/compose-miuix-ui/miuix/tree/main/example)
- Referência para a implementação do picture-in-picture (PiP): [ClassicOldSong/moonlight-android](https://github.com/ClassicOldSong/moonlight-android)
- Abertura das páginas de configurações nativas do aplicativo: [YifePlayte/WOMMO](https://github.com/YifePlayte/WOMMO)
- Implementação do terminal: [reapercanuk39/termux-kotlin-app](https://github.com/reapercanuk39/termux-kotlin-app) (somente a parte sob Apache 2.0)
- Geração de códigos QR: [nayuki/QR-Code-generator](https://github.com/nayuki/QR-Code-generator/tree/master/java)

## Licença

[Apache License 2.0](LICENSE)

## Histórico de estrelas

<a href="https://www.star-history.com/?repos=Miuzarte%2FScrcpyForAndroid&type=date&legend=top-left">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/chart?repos=Miuzarte/ScrcpyForAndroid&type=date&theme=dark&legend=top-left&sealed_token=ZAxkizLKrqW0OrnbwXmuzTskU0mzMsjF--hGG8WW4F38bJGglf17mqXYZ6aQvePlP7ocCCS39PHNQYgjyLIEGcbU_8qQYXZ-YPs5N8slD0MphyJmujabc0AUKWMIpdq6iqSGifrLx-rQGBd26YTwEPikYV6SKjGVAxPhoMmMgzyJ13RtkP3rSm4-E2sN" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/chart?repos=Miuzarte/ScrcpyForAndroid&type=date&legend=top-left&sealed_token=ZAxkizLKrqW0OrnbwXmuzTskU0mzMsjF--hGG8WW4F38bJGglf17mqXYZ6aQvePlP7ocCCS39PHNQYgjyLIEGcbU_8qQYXZ-YPs5N8slD0MphyJmujabc0AUKWMIpdq6iqSGifrLx-rQGBd26YTwEPikYV6SKjGVAxPhoMmMgzyJ13RtkP3rSm4-E2sN" />
   <img alt="Star History Chart" src="https://api.star-history.com/chart?repos=Miuzarte/ScrcpyForAndroid&type=date&legend=top-left&sealed_token=ZAxkizLKrqW0OrnbwXmuzTskU0mzMsjF--hGG8WW4F38bJGglf17mqXYZ6aQvePlP7ocCCS39PHNQYgjyLIEGcbU_8qQYXZ-YPs5N8slD0MphyJmujabc0AUKWMIpdq6iqSGifrLx-rQGBd26YTwEPikYV6SKjGVAxPhoMmMgzyJ13RtkP3rSm4-E2sN" />
 </picture>
</a>

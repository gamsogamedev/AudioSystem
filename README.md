# Como utilizar o sistema

* Tenha o prefab de **SoundManager** na hierarquia da cena
* Tenha no seu código um **SoundData** que vai armazenar os dados do audio
* Crie um **Scriptable Object** de SoundData em `Create -> AudioSystem -> SoundData`

<p align="center">
  <img width="800" alt="SO Path" src="https://github.com/user-attachments/assets/ee76a45e-52be-4375-90cc-92531d0aa06e" />
</p>

No inspetor, você terá essas informações sobre o ScriptableObject

<p align="center">
  <img width="500" alt="SO Info" src="https://github.com/user-attachments/assets/0be57206-b89a-4b1d-8d24-9f093a19afdd" />
</p>

1. `Audio Clip`: Arquivo de audio .wav
2. `FrequentSound`: Sons que são marcados como frequentes caem em uma fila separada e menor que a principal.
3. `Audio Mixer Group`: Qual canal de audio que deve ser utilizado
4. `Loop`: Vai loopar infinitamente em um SoundEmitter, lembre-se de guardar a referencia do SoundEmitter para pará-lo manualmente em código

```csharp
    // SE FOR USAR LOOP E QUISER PARAR O AUDIO EM ALGUM MOMENTO
    //Guarde a referencia do SoundEmitter
        private SoundEmitter musicEmitter;
        private SoundData loopData;
        
        //Crie o audio com PlayOnSoundEmitter
        musicEmitter = SoundManager.Instance.CreateSound().PlayOnSoundEmitter(loopData);
        
        //Depois pare o emitter
        if (musicEmitter != null)
        {
            musicEmitter.Stop();
            musicEmitter =  null;
        }
```
5. `Pitch`: Caso queira mudar a entonação para aquele Scriptable Object
6. `Settings 3D`: Caso queira um audio 3D, você pode ligar spacialBlend e modificar a distância mínima e máxima do audio, assim como o modo de transição dele.

* No geral, audios que são tocados só uma vez são mais simples. Utilize a instancia do SoundManager para tocar o SoundData em um SoundEmitter
```csharp
    private SoundData soundData;
    SoundManager.Instance.CreateSound().Play(soundData);
```
### 

Você pode fazer algumas modificações no audio antes de tocá-lo com código.
```csharp
    SoundManager.Instance.CreateSound().SetPosition(transform).WithRandomPitch().Play(soundData);
```
1. `SetPosition`: Caso esteja utilizando audios 3D, pode settar a posição do emitter
2. `WithRandomPitch`: Se o audio for muito repetitivo, você pode variar um pouco a sua entonação a cada chamada.

#### Na pasta Examples você encontra mais exemplos de como usar o sistema.
# Estrutura das pastas
```text
Assets/AudioSystem/
├── Editor/
│   └── SoundDataEditor.cs
├── Examples/
│   ├── Scenes/
│   │   └── AutoClickExample.unity
│   │   └── SettingsExample.unity
│   └── SoundData/
├── Resources/
│   ├── Prefabs/
│   │   ├── SoundManager.prefab
│   │   └── SoundEmitter.prefab
│   └── MainMixer.mixer
└── Scripts/
│   ├── Settings/
│   │   ├── AudioSettings.cs
│   │   └── ConversionUtils.cs
│   ├── Snapshot/
│   │   ├── SnapshotActions.cs
│   │   └── SnapshotFilter.cs
│   └── SoundData/
│   │   ├── SoundBuilder.cs
│   │   ├── SoundData.cs
│   │   ├── SoundEmitter.cs
│   │   └── SoundManager.cs
```

## Examples
___
Possui alguns ScriptableObjects de exemplo e algumas cenas mostrando como usar o sistema.

### AutoClickExample
* Mostra como pooling de objetos funciona.
* Mesmo com múltiplas chamadas de audio, o consumo de memória é baixo.

Em Playmode, você pode encontrar o prefab do SoundManager em DontDestroyOnLoad e 
ver os SoundEmitter sendo criados, ativados e desativados.

<p align="center">
  <img width="800" alt="SoundManager" src="https://github.com/user-attachments/assets/f6c28ca4-0e49-4494-a28a-fc5496b52e70" />
</p>

#### Dados no inspetor do SoundManager
1. `Collection Check`: A Unity faz uma "remoção dupla" da pool, é pra garantir que bugs não ocorram e caso ocorram, você consiga debugar mais facil.
2. `Default Capacity`: Pré-alocação de memória, se o seu jogo não chegar a usar 10 audios ao mesmo tempo sinta-se a vontade para diminuir.
3. `Max Pool Size`: Quantidade máxima de emitters, a Unity por default não aguenta mais que 32 faixas de audio simultâneas, pesquise mais sobre antes de mudar esse valor.
4. `Max Frequent Instances`: Sons frequentes ocupam um menor espaço do total da pool, isso serve pra você não engolir todos os audios do seu jogo com tiros ou passos do jogador.

### SettingsExample
* Mostra como você pode implementar configurações de audio mais robustas.
* Também utiliza uma conversão de float para decibéis nos sliders, fazendo o som aumentar e diminuir linearmente.

<p align="center">
  <img width="800" alt="SoundMixer" src="https://github.com/user-attachments/assets/304a4697-8d4a-47c4-abd5-8de276b73c70" />
</p>

Caso você não tenha a janela do AudioMixer no seu editor, você pode encontrá-la em `Window -> Audio -> AudioMixer` ou apertando `Ctrl + F8`.

## Resources
___
Aqui você vai encontrar o AudioMixer usado pelo projeto, além dos prefabs de SoundEmitter e do SoundManager.

##### **Você precisa ter o `SoundManager` na hierarquia da cena para o audio funcionar, o SoundEmitter será instanciado pelo manager, você não o coloca diretamente na cena.**

## Scripts
___

### SoundData
* `SoundManager.cs`: Responsável por criar as pools e chamar os emitters.
* `SoundBuilder`: Responsável por construir o emitter.
* `SoundEmitter` Responsável por tocar o audio em si.
* `SoundData`: ScriptableObject que armazena os dados do audio.

### Snapshot
* `SnapshotFilter`: Responsável por fazer a transição de snapshots do AudioMixer
* `SnapshotActions`: Actions para chamar as transições de snapshots

### Settings
* `AudioSettings`: Sistema base que podem utilizar para controlar o volume
* `ConversionUtils`: Cálculo de conversão de float pra decibéis e vice-versa.



## Editor
___
Apenas simplifica o uso dos ScriptableObjects de **SoundData**, as opções 3D não aparecem caso você não use 3D.
Não precisa se preocupar com essa pasta.

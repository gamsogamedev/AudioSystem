# Como utilizar o sistema

* Tenha o prefab de **SoundManager** na hierarquia da cena
* Tenha no seu código um **SoundData** que vai armazenar os dados do audio
* Crie um **Scriptable Object** de SoundData em `Create -> AudioSystem -> SoundData`

<p align="center">
  <img src="https://private-user-images.githubusercontent.com/97367138/573961896-2ecf406d-ebe9-4fa0-a92f-68cd7803c385.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NzU0MzQ3NTMsIm5iZiI6MTc3NTQzNDQ1MywicGF0aCI6Ii85NzM2NzEzOC81NzM5NjE4OTYtMmVjZjQwNmQtZWJlOS00ZmEwLWE5MmYtNjhjZDc4MDNjMzg1LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA0MDYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNDA2VDAwMTQxM1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWYxZTA3NDk2NmM3OGQ4ZmZlMTNiZmE4NGU3NjgyYTkyY2E1ZjZhMmQyN2QzMGRmYWRhY2YxOTdlMmI2NDQ1NGMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.Ge3vJZCS6itJFqkalnRWlIwYAYtepgPpdnv0cTqjPlY" width="600">
</p>

No inspetor, você terá essas informações sobre o ScriptableObject

<p align="center">
  <img src="https://private-user-images.githubusercontent.com/97367138/573961899-c3073967-6b32-49eb-bc4a-4b4d0f86844a.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NzU0MzQ3NTMsIm5iZiI6MTc3NTQzNDQ1MywicGF0aCI6Ii85NzM2NzEzOC81NzM5NjE4OTktYzMwNzM5NjctNmIzMi00OWViLWJjNGEtNGI0ZDBmODY4NDRhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA0MDYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNDA2VDAwMTQxM1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTZhMmJjOWVhYTk5OTk4ZjUxMGQzM2VhZjMzMzYyYjQ1MTEyYzFhN2I3MDZmNDZkYjM2OTc1YmJjZTY4NzNhMTkmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.T9rznxmtTH7oWsKBrXfrNKLbzEjW3JwbOmDRBgCB9V0" width="600">
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
  <img src="https://private-user-images.githubusercontent.com/97367138/573961846-51022d54-62a0-41eb-ba29-edc91a46245f.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NzU0MzQ3NTMsIm5iZiI6MTc3NTQzNDQ1MywicGF0aCI6Ii85NzM2NzEzOC81NzM5NjE4NDYtNTEwMjJkNTQtNjJhMC00MWViLWJhMjktZWRjOTFhNDYyNDVmLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA0MDYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNDA2VDAwMTQxM1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWQzZDhjNTliMDliYThlOTIxMWY4ZjVkMDM2MmE4ZTJhNjgxYzE3YTg3MDhjYjIyYTYxYzY2ZmQ0ZTJmYjJiZmQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.cDTrL6V9bf4RGtvfJLwlIBGXjF6HdGfR22jTiB4hrKE" alt="Demonstração do ScriptableObject" width="600">
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
  <img src="https://private-user-images.githubusercontent.com/97367138/573961892-48964baf-d4a9-4a65-a031-299ca0c85ccd.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NzU0MzQ3NTMsIm5iZiI6MTc3NTQzNDQ1MywicGF0aCI6Ii85NzM2NzEzOC81NzM5NjE4OTItNDg5NjRiYWYtZDRhOS00YTY1LWEwMzEtMjk5Y2EwYzg1Y2NkLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA0MDYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNDA2VDAwMTQxM1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTI1MjBhM2FkYzRjY2Y5NjBjMDBkYTg3NTc2YzI4NTU1YWM5ODliMjE1Yjg3MWU1YzBlODI2OWQxZTAwMWI3OWUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.MyaFH5yZtK-DEM9lnA6E1dDQ_cOBlLV4ujkHCy846Ws" alt="Imagem do AudioMixer" width="600">
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

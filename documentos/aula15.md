## Aula 11 - Criação de Jogos Digitais

> Aula 05/05/2022
> Atividades da aula - Câmera - Início do Projeto do Jogo

## Engine Gráfica Unity3D

- [Conteúdo do Curso - Material sugerido](https://docs.unity3d.com/Manual/Materials.html)


### Passo 4: Câmeras
- [x] Câmera
 - Câmera seguindo o jogador
  - Câmera "filha" do Jogador
  - Câmera x Jogador - Script (CameraController)
   - Obtendo posição do jogador e atualizando a posição da câmera (LateUpdate)
- [x] Cenário
 - Criando um limite/fronteira para o plano
 - Criando cubos para limitar o cenário do jogo
 
 🎬
[![material complementar](https://github.com/marcoswagner-commits/projetos_cg/blob/aa3f6a6ace359cfac3b5b9f9758fb9c642fe950b/Capa_Aula_Unity3D.png)](https://www.youtube.com/watch?v=AXoppi02I1s)

#### Script
 ```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraController : MonoBehaviour
{

    public Transform player;
    private Vector3 offset;
    // Start is called before the first frame update
    void Start()
    {
        offset = transform.position - player.position;
    }

    // Update is called once per frame
    void Update()
    {
              
    }

    private void LateUpdate() {
      transform.position = player.position + offset;
    }
}

 
 ``` 

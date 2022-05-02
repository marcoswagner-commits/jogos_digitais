## Aula 06 - Criação de Jogos Digitais

> Aula 14/04/2022 
> Atividades da aula - Programação

## Engine Gráfica Unity3D

- [Conteúdo do Curso - Material sugerido](https://docs.unity3d.com/Manual/Materials.html)


### Passo 2: Programação
- [x] Scripts
 - Criar uma pasta de Scripts
 - Criar um script
 - Vincular script ao componente
  - Abrir script no Visual Studio Code
  - Analisar o código
   - name_spaces; classes : MonoBehaviour; funções (Start e Update) 
- [x] Variáveis
- [x] Funções
- [x] Controle (if-else - loops)

🎬
[![material complementar](https://github.com/marcoswagner-commits/projetos_cg/blob/aa3f6a6ace359cfac3b5b9f9758fb9c642fe950b/Capa_Aula_Unity3D.png)](https://www.youtube.com/watch?v=jGbjqzE5cH8)

#### Script
 ```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Jogador : MonoBehaviour
public class jogador : MonoBehaviour
{
    int varA = 10;
    int varB = 15;
    // Start is called before the first frame update
    void Start()
    {
        UnityEngine.Debug.Log(Soma(varA,varB));

    if (Soma(varA, varB) > 10) UnityEngine.Debug.Log("Número maior que 10");
    else UnityEngine.Debug.Log("Número menor ou igual a 10");
    for (int i=0; i<10; i++)
        {
          UnityEngine.Debug.Log(i);
        }
        
    }

    int Soma(int a, int b) 
    {
      return (a + b);
    }

    // Update is called once per frame
    void Update()
    {
        
    }
}
 ```


	

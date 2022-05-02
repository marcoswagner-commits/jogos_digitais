## Aula 13 - Criação de Jogos Digitais

> Aula 12/05/2022
> Atividades da aula - Interação - Sequência do Projeto (Caçadores de Ouro)

## Engine Gráfica Unity3D

- [Conteúdo do Curso - Material sugerido](https://docs.unity3d.com/Manual/Materials.html)

### Roteiro Caçadores de Ouro
- [Roteiro Jogo Caçadores de Ouro](https://github.com/marcoswagner-commits/jogos_digitais/tree/documentos/documentos/cacadores_ouro.md)


### Passo 5: Interação
- [x] Colocando texturas
 - Colocar textura para o jogador
 - Colocar textura para o chão
- [x] Criando outros itens
  - Criar um objeto com o nome item (cube)
  - Definir material para o objeto
  - Criando animação via script para os itens
  - Usando o deltaTime para controlar a animação
  - Criando um efeito para o item
- [x] Prefab 
   - Criando uma pasta Prefabs
   - Duplicando os itens
   - Tirando a colisão dos itens (Is Trigger)
   - Habilitando gatilho para os itens
- [x] Colisão
  - Criar método onTriggerEnter para colisão do jogador com os itens
- [x] Objeto via tag
  - Identificando o objeto via tag
  - Criando uma nova tag
  - Comparando uma tag encontrada ("CompareTag")
- [x] Criando um efeito para o jogador (Trail Renderer)


  🎬
[![material complementar](https://github.com/marcoswagner-commits/projetos_cg/blob/aa3f6a6ace359cfac3b5b9f9758fb9c642fe950b/Capa_Aula_Unity3D.png)](https://www.youtube.com/watch?v=nWZuEtOQCg4)
 
#### Script Jogador
 ```
using System.Globalization;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Jogador : MonoBehaviour
{
    Rigidbody rg;

    public float velocidade;

    public GameObject Item_Particula;
     
    // Start is called before the first frame update
    void Start()
    {
       rg = GetComponent<Rigidbody>();
       
    }

     // Update is called once per frame
    void Update()
    {
        
    }

    private void FixedUpdate() 
    {
      float horizontal = Input.GetAxis("Horizontal");
      float vertical = Input.GetAxis("Vertical");
      Vector3 movimento =  new Vector3(horizontal,0,vertical);
      rg.AddForce( movimento * velocidade);
    }

    private void OnTriggerEnter(Collider other) {
      if (other.gameObject.CompareTag("item")) {
        Instantiate(Item_Particula, other.gameObject.transform.position, Quaternion.identity);
        Destroy(other.gameObject);
      }
    }
}

 
 ``` 
 
 #### Script GiraItem
 ```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GiraItem : MonoBehaviour
{
    // Update is called once per frame
    void Update()
    {
        transform.Rotate(new Vector3(15,30,45) * Time.deltaTime);
    }
}

 
 ```
 


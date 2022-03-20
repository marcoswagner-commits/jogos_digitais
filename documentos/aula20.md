## Aula 20 - Criação de Jogos Digitais

> Aula 23/06/2022
> Atividades da aula - Animação

## Engine Gráfica Unity3D

- [Conteúdo do Curso - Material sugerido](https://docs.unity3d.com/Manual/Materials.html)


### Passo 7: Animação
- [x] Import de Assets
  - https://assetstore.unity.com
  - Importar via Package Manager (Jammo_LowPoly)
- [x] Configuração de Animações
  - Associar Animator_Controller_Jamo ao Personagem
  - Abrir Animator (Window - Animation - Animator)
  - Colocar as animações Idle, Running, Victory Idle
  - Excluir outras animações
  - Criar transição entre Idle e Running (ida e volta) - Make Transition
  - Criar parâmetro "run" e aplicar nas condições das transições
  - Desmarcar opção Has Exit Time
- [x] Animação via script
  - Transformar o personagem em Jogador
  - Adicionar script Jogador - componente Rigidbody - componente Capsule Collider (ajustar...)
  - Adequar o código do jogador para movimento (parado e correndo)
  - Adequar orientação do jogador (usar Quaternion.LookRotation)

🎬
[![material complementar](https://github.com/marcoswagner-commits/projetos_cg/blob/aa3f6a6ace359cfac3b5b9f9758fb9c642fe950b/Capa_Aula_Unity3D.png)](https://www.youtube.com/watch?v=dHwMjHzQ7n8)
 
 #### Script Jogador
 ```
using System.Runtime.InteropServices;
using System.Globalization;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Jogador : MonoBehaviour
{
    Rigidbody rg;
    Animator animator;
    public float velocidade;
    public GameObject Item_Particula;
    
      
    // Start is called before the first frame update
    void Start()
    {
       rg = GetComponent<Rigidbody>();
       animator = GetComponent<Animator>();
       
    }

     // Update is called once per frame
    void Update()
    {
        
    }

    private void FixedUpdate() 
    {
      float horizontal = Input.GetAxis("Horizontal");
      float vertical = Input.GetAxis("Vertical");

      if (horizontal  != 0 || vertical != 0) 
      {
          animator.SetBool("run", true);
      }
      else
      {
          animator.SetBool("run", false);
      }

      Vector3 movimento =  new Vector3(horizontal,0,vertical);

      if (movimento != Vector3.zero)
        transform.rotation = Quaternion.LookRotation(movimento);

      rg.AddForce( movimento * velocidade);
    }

    private void OnTriggerEnter(Collider other) {
      if (other.gameObject.CompareTag("item")) {
        Instantiate(Item_Particula, other.gameObject.transform.position, Quaternion.identity);
        Destroy(other.gameObject);
        NivelController.instance.SetItensColetados();
      }
    }
}


 ``` 

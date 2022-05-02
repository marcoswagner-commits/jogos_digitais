## Aula 19 - Criação de Jogos Digitais

> Aula 09/06/2022
> Atividades da aula - Inimigos e NavMesh - Sequência do Projeto (Caçadores de Ouro)

## Engine Gráfica Unity3D

- [Conteúdo do Curso - Material sugerido](https://docs.unity3d.com/Manual/Materials.html)

### Roteiro Caçadores de Ouro
- [Roteiro Jogo Caçadores de Ouro](https://github.com/marcoswagner-commits/jogos_digitais/tree/documentos/documentos/cacadores_ouro.md)


### Passo 8: Inimigo e NavMesh
- [x] Importando um "inimigo"
 - https://assetstore.unity.com
  - Importar via Package Manager (Fantasy Monster - Skeleton)
  - Escolher as animações para jogo
  - Colocar o personagem na hierarquia (configurar o posicionamento e tamanho) - Tirar o Prefab - Criar um AnimatorController 
- [x] NavMesh
  - Abrir Navigation (Window - AI - Navigation)
  - Preparar o ambiente
  - Selecionar o Piso (chão) e Paredes como estáticos 
  - Na janela Navigation - Bake - Baker (área onde o inimigo irá percorrer)
  - Adicionar ao "inimigo" o component Nav Mesh Agent
- [x] Comportamentos
  - Criar um script "inimigo"
   - Criar um vector de posições (Transform[])
   - Verificar o método SetDestination
   - Criar um conjunto de pontos no cenário (Points)
- [x] Configurar jogador para comemorar vitória
- [x] Configurar inimigo para atacar

🎬
[![material complementar](https://github.com/marcoswagner-commits/projetos_cg/blob/aa3f6a6ace359cfac3b5b9f9758fb9c642fe950b/Capa_Aula_Unity3D.png)](https://www.youtube.com/watch?v=uwji4t-8IoA)

 
#### Script Jogador
 ```
using System.Runtime.InteropServices;
using System.Globalization;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Jogador : MonoBehaviour
{
   private Rigidbody rg;
   public float velocidade;
   public GameObject Item_Particula;
   private Animator an;
   private bool isDead;
   
     
   // Start is called before the first frame update
   void Start()
   {
      rg = GetComponent<Rigidbody>();
      an = GetComponent<Animator>();
      
   }

    // Update is called once per frame
   void Update()
   {
       
   }

   private void FixedUpdate() 
   {

     if (isDead) return; 

     float horizontal = Input.GetAxis("Horizontal");
     float vertical = Input.GetAxis("Vertical");

      if(horizontal != 0 || vertical != 0) 
      {
        an.SetBool("run",true);
      }
      else
      {
        an.SetBool("run",false);
      }

      Vector3 movimento =  new Vector3(horizontal,0,vertical);

      if (movimento != Vector3.zero) 
        {
          transform.rotation = Quaternion.LookRotation(movimento);
        }
 
      rg.AddForce( movimento * velocidade);
   }

   private void OnTriggerEnter(Collider other) {
     if (other.gameObject.CompareTag("Item")) {
       Instantiate(Item_Particula, other.gameObject.transform.position, Quaternion.identity);
       Destroy(other.gameObject);
       NivelController.instance.SetItensColetados();
       if (NivelController.instance.GetWin()) Win();
      }
      else if (other.gameObject.CompareTag("inimigo")) {
        Instantiate(Item_Particula, other.gameObject.transform.position, Quaternion.identity);
        Death();
      }
       
   }

   private void Death() {
     isDead = true;
     Destroy(gameObject);
   }

   private void Win() {
     
     an.SetBool("win",true);
   }
}
 ```  
 
 #### Script Inimigo
 ```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;

public class Inimigo : MonoBehaviour
{
    public Transform[] destPoints;
    private NavMeshAgent agent;
    private Animator animator;
    private int index = 0;
    // Start is called before the first frame update
    void Start()
    {
      animator = GetComponent<Animator>();
      agent = GetComponent<NavMeshAgent>();
      agent.SetDestination(destPoints[index].position);
        
    }

    // Update is called once per frame
    void Update()
    {
        if (agent.remainingDistance < 0.5f) {
          index++;
          if (index >= destPoints.Length ) index = 0;
          agent.SetDestination(destPoints[index].position);
        }
    }

    private void OnTriggerEnter(Collider other) {
     if (other.gameObject.CompareTag("jogador")) {
       animator.SetBool("attack",true);
      }
       
   }
}

 ```  
 
 #### Script NivelController
 ```
using System.Diagnostics;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class NivelController : MonoBehaviour
{
   public static NivelController instance;

   public int totalitens;

   public Text textoPontos;
   public Text textoTotal;
   public GameObject textoFinal;
   private bool win;

   private int itenscoletados;

  void Awake()
   {
     instance = this;
       
   }
  // Start is called before the first frame update
   void Start()
   {
       textoFinal.SetActive(false);
       textoPontos.text = "Itens Coletados: 0" + itenscoletados;
       win=false;
   }

   public void SetItensColetados() 
   {
     itenscoletados++;

     textoPontos.text = "Itens Coletados: 0" + itenscoletados.ToString();

     if (itenscoletados >= totalitens) {textoFinal.SetActive(true); win=true;}
     
     
   }

   public bool GetWin() 
   {
     return win;
          
   }

   // Update is called once per frame
   void Update()
   {
       
   }
}
 ```  

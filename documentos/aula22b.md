## Aula 22 - Criação de Jogos Digitais

> Aula 23/06/2022
> Atividades da aula - Audios - Sequência do Projeto (Caçadores de Ouro)

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
 
### Passo 9: Audios
- [x] Audio Source 
 - Importando o audio de um pacote
 - Criar um objeto vazio
 - "Resetar" o transform
 - Colocar um componente "Audio Source"
 - Vincular o audio "ambiente" ao componente
 - Diminuir o volume
 - Marcar a opção Play on Awake
 - Marcar a opção Loop
- [x] Audio via script
  - Criar um componente no script do jogador do tipo AudioSource (GetComponent - audioSource)
  - Criar uma variável pública do tipo AudioClip para três áudios (itens coletados, vitória e morte)
  - Criar uma função para tocar o audio (PlayAudio - AuditoClip clip)
   - audioSource.clip = clip // auditoSource.Play();
  - Criar um vínculo com os métodos de vitória, morte, itens coletados
 
 🎬
[![material complementar](https://github.com/marcoswagner-commits/projetos_cg/blob/aa3f6a6ace359cfac3b5b9f9758fb9c642fe950b/Capa_Aula_Unity3D.png)](https://www.youtube.com/watch?v=UUmpPt_xg8U)

 
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
    private Animator animator;
    private AudioSource audio;
    public float velocidade;
    public GameObject Item_Particula;
    private  bool isDead;
    public AudioClip itens, win, dead;
    
      
    // Start is called before the first frame update
    void Start()
    {
       rg = GetComponent<Rigidbody>();
       animator = GetComponent<Animator>();
       audio = GetComponent<AudioSource>();
       
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
        PlayAudio(itens);
        if (NivelController.instance.GetWin()) Win();
      }
      else if (other.gameObject.CompareTag("Inimigo")) {
        Instantiate(Item_Particula, other.gameObject.transform.position, Quaternion.identity);
        Death();
      }
    }

    private void Death() {
      isDead = true;
      Destroy(gameObject);
      PlayAudio(dead);
    }

    private void Win() {
      animator.SetBool("win", true);
      PlayAudio(win);
    }

    private void PlayAudio(AudioClip clip) {
      audio.clip = clip;
      audio.Play();
    }
}

```  

### Passo 10: Build
- [x] Controle de cena
 - Criar um script SceneController
 - Criar uma função pública LoadScene(string scene)
 - Importar a classe SceneManagement
 - Criar uma função pública ReloadScene(SceneManager.GetActive Scene(). buildIndex);
 - Transformar a classe principal para Singleton
 - Chamar a classe em jogador
- [x] Build do Jogo

 
 🎬
[![material complementar](https://github.com/marcoswagner-commits/projetos_cg/blob/aa3f6a6ace359cfac3b5b9f9758fb9c642fe950b/Capa_Aula_Unity3D.png)](https://www.youtube.com/watch?v=q0mDs_V55KE)

 
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
    private Animator animator;
    private AudioSource audioSource;
    public float velocidade;
    public GameObject Item_Particula;
    private  bool isDead;
    public AudioClip itens, win, dead;
    public  string scene;
    
      
    // Start is called before the first frame update
    void Start()
    {
       rg = GetComponent<Rigidbody>();
       animator = GetComponent<Animator>();
       audioSource = GetComponent<AudioSource>();
       
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
        PlayAudio(itens);
        if (NivelController.instance.GetWin()) Win();
      }
      else if (other.gameObject.CompareTag("Inimigo")) {
        Instantiate(Item_Particula, other.gameObject.transform.position, Quaternion.identity);
        Death();
      }
    }

    private void Death() {
      isDead = true;
      PlayAudio(dead);
      this.gameObject.SetActive(false);
      Invoke("ReloadScene", 3F);
      
    }

    private void Win() {
      animator.SetBool("win", true);
      PlayAudio(win);
      Invoke("LoadScene", 3F);
    }

    private void PlayAudio(AudioClip clip) {
      audioSource.clip = clip;
      audioSource.Play();
    }

    private void RelaoadScene() {
      SceneController.instance.ReloadScene();
    }

    private void LoadScene() {
      SceneController.instance.LoadScene(scene);
    }
}


``` 
 
 
#### Script SceneController
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class SceneController : MonoBehaviour
{
    public static SceneController instance;

    private void Awake() {
      instance = this;
    }

    public void LoadScene(string scene) {
      SceneManager.LoadScene(scene);
    }

    public void ReloadScene() {
      SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
    }
}


```

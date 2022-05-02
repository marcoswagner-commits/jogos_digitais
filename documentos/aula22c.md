## Aula 24 - Criação de Jogos Digitais

> Aula 30/06/2022
> Atividades da aula - Incrementos - Build - Conclusão do Projeto (Caçadores de Ouro)

## Engine Gráfica Unity3D

- [Conteúdo do Curso - Material sugerido](https://docs.unity3d.com/Manual/Materials.html)

### Roteiro Caçadores de Ouro
- [Roteiro Jogo Caçadores de Ouro](https://github.com/marcoswagner-commits/jogos_digitais/tree/documentos/documentos/cacadores_ouro.md)



### Passo 10: Build
- [x] Incrementos no Projeto (Caveirão e outros)
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

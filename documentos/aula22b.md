## Aula 22 - Criação de Jogos Digitais

> Aula 23/06/2022
> Atividades da aula - Audios - Sequência do Projeto (Caçadores de Ouro)

## Engine Gráfica Unity3D

- [Conteúdo do Curso - Material sugerido](https://docs.unity3d.com/Manual/Materials.html)

### Roteiro Caçadores de Ouro
- [Roteiro Jogo Caçadores de Ouro](https://github.com/marcoswagner-commits/jogos_digitais/tree/documentos/documentos/cacadores_ouro.md)


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
  - Criar uma função para tocar o audio (PlayAudio - AudioClip clip)
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



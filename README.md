# Rapatila
using UnityEngine;
using UnityEngine.SceneManagement;

public class MainMenu : MonoBehaviour
{
    public GameObject mainMenuUI;

    // Başla butonuna basıldığında
    public void StartGame()
    {
        SceneManager.LoadScene("GameScene"); // Oyun sahnesine geçiş
    }

    // Çıkış butonuna basıldığında
    public void QuitGame()
    {
        Application.Quit();
    }
}using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public Camera playerCamera;
    public Transform bed;
    public float lookSpeed = 2f;

    private bool lookingAtBed = false;

    void Update()
    {
        // Yatakta olup olmadığını kontrol et
        if (Vector3.Distance(transform.position, bed.position) < 2f)
        {
            lookingAtBed = true;
        }
        else
        {
            lookingAtBed = false;
        }

        // Eğer yatakta ise bakmak için tuşu kullanabiliriz
        if (lookingAtBed && Input.GetKeyDown(KeyCode.F))
        {
            LookUnderBed();
        }
    }

    void LookUnderBed()
    {
        // Yatak altını görmek için ışık açma
        Debug.Log("Fener Açıldı");
    }
}using UnityEngine;

public class Monster : MonoBehaviour
{
    public Transform target;  // Oyuncu veya odanın belirli alanları
    public float moveSpeed = 1f;
    public bool isVisible = false;  // Yaratık görünür mü?

    void Update()
    {
        if (isVisible)
        {
            MoveTowardsPlayer();
        }
    }

    void MoveTowardsPlayer()
    {
        if (target != null)
        {
            transform.position = Vector3.MoveTowards(transform.position, target.position, moveSpeed * Time.deltaTime);
        }
    }

    // Yaratık görünür olduğunda oyuncuya saldırabilir
    public void Appear()
    {
        isVisible = true;
    }

    // Yaratık kaybolduğunda tekrar gizlenebilir
    public void Disappear()
    {
        isVisible = false;
    }
}using UnityEngine;

public class PhoneCamera : MonoBehaviour
{
    public Camera camera;
    public GameObject flashLight;  // Telefon ışığı
    public bool isFlashing = false;

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.C))  // Kamera açma tuşu
        {
            TakePhoto();
        }

        if (Input.GetKeyDown(KeyCode.F))  // Telefon ışığı açma tuşu
        {
            ToggleFlash();
        }
    }

    void TakePhoto()
    {
        // Fotoğraf çekme işlemi
        Debug.Log("Fotoğraf Çekildi");
        // Yaratıkları fotoğraflamak gibi bir işlem eklenebilir
    }

    void ToggleFlash()
    {
        isFlashing = !isFlashing;
        flashLight.SetActive(isFlashing);  // Işık açılır kapanır
    }
}using UnityEngine;
using UnityEngine.UI;

public class GameTimeManager : MonoBehaviour
{
    public Text clockText;  // Saat göstergesi
    public float gameTime = 0f;
    public AudioSource creepySounds;  // Korkutucu sesler

    void Update()
    {
        gameTime += Time.deltaTime;

        int hour = (int)(gameTime / 3600);  // Saat
        int minute = (int)((gameTime % 3600) / 60);  // Dakika

        // Saatin 3'ten sonra korkunç sesler çalmaya başlar
        if (hour >= 3)
        {
            creepySounds.Play();  // Korkutucu ses çalar
        }

        clockText.text = string.Format("{0:D2}:{1:D2}", hour, minute);  // Saat formatı
    }
}using UnityEngine;
using UnityEngine.SceneManagement;

public class GameOverManager : MonoBehaviour
{
    public GameObject gameOverUI;

    void Update()
    {
        if (Time.time >= 6 * 3600)  // Saat 6 olduğunda oyun biter
        {
            GameOver();
        }
    }

    void GameOver()
    {
        gameOverUI.SetActive(true);  // Oyun sonu ekranını göster
        Time.timeScale = 0;  // Oyun durdurulur
    }
}

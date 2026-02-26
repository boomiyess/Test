# Test
Ad 👍 if it's good ad 🤬 if it's not good 
using UnityEngine;

[CreateAssetMenu(fileName = "New Character", menuName = "Game/Character Profile")]
public class CharacterData : ScriptableObject
{
    public string characterName;
    public GameObject bodyMesh;     // The high-poly 3D model
    public RuntimeAnimatorController animations; // Unique move sets
    
    [Header("Base Stats")]
    public float health = 100f;
    public float speedMultiplier = 1.0f;
    
    [Header("Default Gear")]
    public WeaponData startingWeapon;
}using UnityEngine;
using UnityEngine.UI;
using TMPro; // For high-quality text

public class SkinShopManager : MonoBehaviour
{
    public WeaponSkin[] availableSkins; // List of all skins in the game
    public WeaponSkinHandler currentWeapon; // The 3D gun in the preview room
    
    public TextMeshProUGUI skinNameText;
    public TextMeshProUGUI priceText;
    public Button buyButton;

    private int selectedIndex = 0;
    public int playerCurrency = 5000; // Example starting money

    void Start() {
        UpdatePreview();
    }

    public void NextSkin() {
        selectedIndex = (selectedIndex + 1) % availableSkins.Length;
        UpdatePreview();
    }

    public void PreviousSkin() {
        selectedIndex--;
        if (selectedIndex < 0) selectedIndex = availableSkins.Length - 1;
        UpdatePreview();
    }

    void UpdatePreview() {
        WeaponSkin selected = availableSkins[selectedIndex];
        
        // 1. Change the 3D Model's look instantly
        currentWeapon.ApplySkin(selected);

        // 2. Update the UI Text
        skinNameText.text = selected.skinName;
        priceText.text = "$" + selected.price.ToString();

        // 3. Check if player can afford it
        buyButton.interactable = (playerCurrency >= selected.price);
    }

    public void BuySkin() {
        WeaponSkin selected = availableSkins[selectedIndex];
        if (playerCurrency >= selected.price) {
            playerCurrency -= selected.price;
            Debug.Log("Purchased: " + selected.skinName);
            // Save to player's permanent inventory here
        }
    }
}using UnityEngine;

[CreateAssetMenu(fileName = "New Weapon", menuName = "Weapon System/Weapon")]
public class WeaponData : ScriptableObject
{
    public string weaponName;
    public GameObject weaponPrefab; // The 3D Model
    public float damage;
    public float fireRate;
    public int maxAmmo;
    public float reloadTime;
    
    [Header("Visuals & Audio")]
    public GameObject muzzleFlashPrefab;
    public AudioClip fireSound;
    public Vector3 hipPosition;    // For high-quality weapon swaying
    public Vector3 adsPosition;    // Aim Down Sights position
}using UnityEngine;

public class HighEndShooter : MonoBehaviour
{
    public float damage = 10f;
    public float range = 100f;
    public float fireRate = 15f;
    public float impactForce = 30f;

    public Camera fpsCam;
    public ParticleSystem muzzleFlash;
    public GameObject impactEffect; // High-quality spark/dust prefab

    private float nextTimeToFire = 0f;

    void Update()
    {
        // Check for Left Mouse Button and Fire Rate
        if (Input.GetButton("Fire1") && Time.time >= nextTimeToFire)
        {
            nextTimeToFire = Time.time + 1f / fireRate;
            Shoot();
        }
    }

    void Shoot()
    {
        muzzleFlash.Play();

        RaycastHit hit;
        // Shoot a ray from the center of the camera forward
        if (Physics.Raycast(fpsCam.transform.position, fpsCam.transform.forward, out hit, range))
        {
            Debug.Log("Hit: " + hit.transform.name);

            // 1. Apply Damage (if the object has a Health script)
            Target target = hit.transform.GetComponent<Target>();
            if (target != null) {
                target.TakeDamage(damage);
            }

            // 2. Apply Physics Force (High-quality interaction)
            if (hit.rigidbody != null) {
                hit.rigidbody.AddForce(-hit.normal * impactForce);
            }

            // 3. Spawn Visual Impact (Sparks, holes, etc.)
            GameObject impactGO = Instantiate(impactEffect, hit.point, Quaternion.LookRotation(hit.normal));
            Destroy(impactGO, 2f); // Clean up after 2 seconds
        }
    }
}using UnityEngine;

public class CharacterLookAt : MonoBehaviour
{
    public Animator anim;
    public Transform target; // The point where the player is aiming
    public float lookWeight = 1.0f; // How much the head follows

    void OnAnimatorIK()
    {
        if (anim)
        {
            // Set the look-at position for the head/eyes
            anim.SetLookAtWeight(lookWeight);
            anim.SetLookAtPosition(target.position);
        }
    }
}using UnityEngine;

[CreateAssetMenu(fileName = "New Weapon", menuName = "Weapon System/Weapon")]
public class WeaponData : ScriptableObject
{
    public string weaponName;
    public GameObject weaponPrefab; // The 3D Model
    public float damage;
    public float fireRate;
    public int maxAmmo;
    public float reloadTime;
    
    [Header("Visuals & Audio")]
    public GameObject muzzleFlashPrefab;
    public AudioClip fireSound;
    public Vector3 hipPosition;    // For high-quality weapon swaying
    public Vector3 adsPosition;    // Aim Down Sights position
}hit.transform.nameusing UnityEngine;

public class CharacterLookAt : MonoBehaviour
{
    public Animator anim;
    public Transform target; // The point where the player is aiming
    public float lookWeight = 1.0f; // How much the head follows

    void OnAnimatorIK()
    {
        if (anim)
        {
            // Set the look-at position for the head/eyes
            anim.SetLookAtWeight(lookWeight);
            anim.SetLookAtPosition(target.position);
        }
    }
}skinNameText.textavailableSkins.LengthavailableSkins.Length

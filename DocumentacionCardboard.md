* Utilizar version de unity igual o posterior a la 2021.3.3221
* Se debe tener instalado git para descargar el paquede de carboard
#### Añadir paquete desde un git url
* https://github.com/googlevr/cardboard-xr-plugin.git


## Pasos seguidos para primer tutorial

* Descargar el Google VR SDK para Unity desde el sitio oficial de Google y sigue las instrucciones para integrarlo en tu proyecto Unity.
* Configurar proyecto para Google Cardboard. Se debe configurar los ajustes de VR en Unity para usar Google VR.

## Lanzar un objeto

```csharp
using UnityEngine;

public class ThrowObject : MonoBehaviour
{
    public GameObject throwableObject;
    public float throwForce = 10f;

    void Update()
    {
        if (GvrPointerInputModule.Pointer.TriggerDown)
        {
            Throw();
        }
    }

    void Throw()
    {
        GameObject thrownObject = Instantiate(throwableObject, transform.position, transform.rotation);
        Rigidbody rb = thrownObject.GetComponent<Rigidbody>();
        if (rb != null)
        {
            rb.AddForce(transform.forward * throwForce, ForceMode.VelocityChange);
        }
    }
}

```

## Recoger un objeto

```csharp
using UnityEngine;

public class PickupObject : MonoBehaviour
{
    public Transform holdPosition;
    private GameObject heldObject = null;

    void Update()
    {
        if (GvrPointerInputModule.Pointer.TriggerDown && heldObject == null)
        {
            RaycastHit hit;
            if (Physics.Raycast(transform.position, transform.forward, out hit, 5f))
            {
                if (hit.transform.CompareTag("Pickup"))
                {
                    heldObject = hit.transform.gameObject;
                    heldObject.transform.position = holdPosition.position;
                    heldObject.transform.parent = holdPosition;
                }
            }
        }
        else if (GvrPointerInputModule.Pointer.TriggerDown && heldObject != null)
        {
            heldObject.transform.parent = null;
            heldObject = null;
        }
    }
}
```
## Rotar un objeto
```csharp
using UnityEngine;

public class RotateObject : MonoBehaviour
{
    public float rotationSpeed = 50f;

    void Update()
    {
        if (GvrPointerInputModule.Pointer.Triggering)
        {
            transform.Rotate(Vector3.up, rotationSpeed * Time.deltaTime);
        }
    }
}

```

## Teletransportarse
```csharp
using UnityEngine;

public class Teleport : MonoBehaviour
{
    public Transform[] teleportLocations;

    void Update()
    {
        if (GvrPointerInputModule.Pointer.TriggerDown)
        {
            int randomIndex = Random.Range(0, teleportLocations.Length);
            transform.position = teleportLocations[randomIndex].position;
        }
    }
}

```


## Google VR SDK


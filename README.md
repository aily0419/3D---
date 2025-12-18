# 3D---
麻將房間
```csharp
using UnityEngine;

public class MahjongRoomCreator : MonoBehaviour
{
    [Header("房間尺寸")]
    public float roomWidth = 10f;  // 房間寬度
    public float roomHeight = 5f;  // 房間高度
    public float roomDepth = 10f;  // 房間深度
    [Header("材質設定 (可選)")]
    public Material wallMaterial;  // 牆壁材質
    public Material floorMaterial; // 地板材質

    void Start()
    {
        CreateRoom();
    }

    void CreateRoom()
    {
        // 建立一個空的 GameObject 作為房間容器
        GameObject roomParent = new GameObject("Mahjong_Room");

        // 1. 地板
        CreateSurface("Floor", new Vector3(0, -0.1f, 0), new Vector3(roomWidth, 0.2f, roomDepth), Color.gray, roomParent);

        // 2. 天花板
        CreateSurface("Ceiling", new Vector3(0, roomHeight, 0), new Vector3(roomWidth, 0.2f, roomDepth), Color.white, roomParent);

        // 3. 四面牆壁
        // 前牆 (North)
        CreateSurface("Wall_N", new Vector3(0, roomHeight/2, roomDepth/2), new Vector3(roomWidth, roomHeight, 0.2f), new Color(0.8f, 0.8f, 0.7f), roomParent);
        // 後牆 (South)
        CreateSurface("Wall_S", new Vector3(0, roomHeight/2, -roomDepth/2), new Vector3(roomWidth, roomHeight, 0.2f), new Color(0.8f, 0.8f, 0.7f), roomParent);
        // 右牆 (East)
        CreateSurface("Wall_E", new Vector3(roomWidth/2, roomHeight/2, 0), new Vector3(0.2f, roomHeight, roomDepth), new Color(0.7f, 0.7f, 0.6f), roomParent);
        // 左牆 (West)
        CreateSurface("Wall_W", new Vector3(-roomWidth/2, roomHeight/2, 0), new Vector3(0.2f, roomHeight, roomDepth), new Color(0.7f, 0.7f, 0.6f), roomParent);

        // 4. 自動加上一盞溫暖的吊燈
        GameObject lightObj = new GameObject("Room_Light");
        Light light = lightObj.AddComponent<Light>();
        light.type = LightType.Point;
        light.range = 20f;
        light.intensity = 1.5f;
        light.color = new Color(1f, 0.95f, 0.8f); // 偏暖色
        lightObj.transform.position = new Vector3(0, roomHeight - 1f, 0);
    }

    void CreateSurface(string name, Vector3 pos, Vector3 scale, Color color, GameObject parent)
    {
        GameObject obj = GameObject.CreatePrimitive(PrimitiveType.Cube);
        obj.name = name;
        obj.transform.position = pos;
        obj.transform.localScale = scale;
        obj.transform.parent = parent.transform;
        
        // 設定顏色，讓牆壁看起來不像預設的白色那麼刺眼
        obj.GetComponent<Renderer>().material.color = color;
    }
}

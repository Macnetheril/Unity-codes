//将生成的对象放在一个文件下，免得杂乱

Transform boardHolder;
boardHolder = new GameObject("BoardHolder").transform;
GameObject instance = Instantiate(toInstantiate, new Vector3(x, y, 0f), Quaternion.identity);
instance.transform.SetParent(boardHolder);

//另一种收束文件的方式大约是这个样子

    position.x = (x + z * 0.5f - z / 2) * (HexMetrics.innerRadius * 2f);
		
		cell.transform.SetParent(transform, false);
		cell.transform.localPosition = position;


//初始化内部坐标集

List<Vector3> gridPositions = new List<Vector3>();

    void InitialiseList()
    {
        gridPositions.Clear();
        for (int x = 1; x < columns - 1;x++)
        {
            for (int y = 1; y < rows - 1;y++)
            {
                gridPositions.Add(new Vector3(x, y, 0f));
            }
        }
    }
        
    
//得到坐标集里的随机坐标  
Vector3 RandomGridPosition()
    {
        int randomIndex = Random.Range(0, gridPositions.Count);
        Vector3 randomGridPosition = gridPositions[randomIndex];
        gridPositions.RemoveAt(randomIndex);
        return randomGridPosition;
    }
    
//平滑地移动，特别注意Vector3.MoveTowards
    IEnumerator SmoothMovement(Vector3 end)
    {
        float sqrRemainingDistance = (transform.position - end).sqrMagnitude;
        while(sqrRemainingDistance > float.Epsilon)
        {
            Vector3 newPosition = Vector3.MoveTowards(rb2D.position,end,inverseMoveTime * Time.deltaTime);
            rb2D.MovePosition(newPosition);
            sqrRemainingDistance = (transform.position - end).sqrMagnitude;
            yield return null;
        }
    }    
    
//重新加载当前场景，当场游戏只有一个main scene    
    if(restart)
        {
            if(Input.GetKeyDown(KeyCode.R))
            {
                SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex, LoadSceneMode.Single);
            }
        }  
    
    
//单曲播放功能-- SoundManager.instance.PlaySingle(gameOverSound);

    public void PlaySingle(AudioClip clip) 
    {
        efxSource.clip = clip;
        efxSource.Play();
    }

//随机播放功能，其中参数加个关键字param，这样就允许相同类型的参数可以直接用逗号分隔添加，它们就会分别传入数组中。
//-- SoundManager.instance.RandomizeSfx(enemyAttack1, enemyAttack2);

    public void RandomizeSfx(params AudioClip[] clips) 
    {
        int randomIndex = Random.Range(0, clips.Length);
        float randomPitch = Random.Range(lowPitchRange, highPitchRange);//随机变化音高

        efxSource.pitch = randomPitch;
        efxSource.clip = clips[randomIndex];
        efxSource.Play();
    }    
    

//摄像机跟随视角设置
    public GameObject player;   
    private Vector3 offset;
   
    void Start()
    {
       offset = transform.position - player.transform.position;
    }    
    void LateUpdate()
    {
        transform.position = player.transform.position + offset;
    }
    

//摄像机跟随另一种写法
    void Start ()
    {
        offset = transform.position - target.position;
    }
    void FixedUpdate ()
    {
        Vector3 targetCamPos = target.position + offset;     
        transform.position = Vector3.Lerp (transform.position, targetCamPos, smoothing * Time.deltaTime);
    }   
    
    
    
 //背景循环滚动
     public float scrollSpeed;
    public float tileSizeZ;
    Vector3 startPostion;

		void Start ()
    {
        startPostion = transform.position;
	  }	
	void Update ()
    {
        float newPosition = Mathf.Repeat(Time.time * scrollSpeed,tileSizeZ);
        transform.position = startPostion + Vector3.forward * newPosition;
	  }
    
    
//将飞机收束在一定范围里，不跑出界面
        rb.position = new Vector3
        (
                Mathf.Clamp(rb.position.x,boundary.xMin,boundary.xMax),
                0.0f,
                Mathf.Clamp(rb.position.z,boundary.zMin,boundary.zMax)
        );
    
    
//子弹向前
GetComponent<Rigidbody>().velocity = transform.forward * speed;


//多个生成点射出子弹
         foreach (var shotSpawn in shotSpawns)
         {
              GameObject instance = Instantiate(shot, shotSpawn.transform.position, shotSpawn.transform.rotation);
              instance.transform.SetParent(shotBolder);
         }
    
    
//陨石随机旋转
GetComponent<Rigidbody>().angularVelocity = Random.insideUnitSphere * tumble;
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    

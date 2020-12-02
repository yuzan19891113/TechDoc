# Coroutine

协程：即允许你将执行过程分摊到每一帧去执行

开启协程：startcoroutine

关闭协程：stopcoroutine

**协程例子**

```text
void Update()
{
    if(Input.GetMouseButtonDown(0))
    {
        StartCoroutine(MoveTank());
    }
}

IEnumerator MoveTank()
    {
        while(facingWrongWay)
        {
            TurnTank();
            yield return null;
        }

        while (notInPosition)
        {
            MoveTank();
            yield return null;
        }

        yield return new WaitForSeconds(1);
        Fire();    
    }
```

**协程可以根据条件来结束**

```text
IEnumerator WaitFiveSeconds()
    {
        // Pause the game
        Time.timeScale = 0;

        yield return new WaitForSecondsRealtime(5);

        print("You can't stop me");
    }
```

**或者根据时间来结束**

```text
int fuel=500;

    void Start()
    {
        StartCoroutine(CheckFuel());
    }

    private void Update()
    {
        fuel--;
    }

    IEnumerator CheckFuel()
    {
        yield return new WaitUntil(IsEmpty);
        print("tank is empty");
    }

    bool IsEmpty()
    {
        if (fuel > 0)
        {
            return false;
        }
        else 
        {
            return true;
        }
    }
```


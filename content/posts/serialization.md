---
title: "직렬화/역직렬화"
date: 2021-06-28T23:09:09+09:00
draft: true
tags:
    - Java
    - Serializable
    - Parcelable
---

## 직렬화/역직렬화

데이터 구조나 객체들을 파일이나 메모리 버퍼에 저장하거나 전송할 수 있는 형태로 변환하는 과정이다.

반대가 역직렬화.

## in Java

객체 → 바이트

자바에서의 직렬화는 객체를 다른 자바 시스템에서도 사용할 수 있도록 byte형태로 변환하는 것이다. 자바에서의 I/O처리는 primitive 타입인 Int, Long 등이나 String을 지원한다. 하지만 실제로 우리가 만들어낸 클래스들은 이런 경우에 바이트로 변환했다가 다시 클래스로 복원해줘야 하는 것이다.

## Serializable

primitive타입이 아니라면 Class를 `Serializable` 인터페이스(java.io.Serializable)를 구현하는 것으로 간단히 직렬화할 수 있다. (이러한 인터페이스를 체크를 위한 Marker 인터페이스라고 한다.)

```kotlin
data class Icecream(
	val price: Int,
	val taste: String
): Serializable
```

> 아무것도 override하지 않고 어떻게 가능한 걸까?

ObjectOutputStream를 통해 writeObject()를 호출해서 객체가 Serializable interface를 구현했는지 JVM는 체크를 한다. 그렇지 않으면 `NotSerializableException`를 던진다.

[[JAVA] 객체 직렬화 ObjectInputStream / ObjectOutputStream](https://hyeonstorage.tistory.com/252)

## in android

안드로이드에서는 A Activity에서 B Activity로 이동하면서 데이터를 전달하는 경우에 많이 보고 사용했을 것이다. `Intent` 에 데이터를 담아서 전달하게 된다.  `Serializable`도 사용이 가능하지만 속도가 빠른 `Parcelable`를 사용하기를 권장하고 있다.

## 그렇다면 왜 이렇게 해야만 할까?

안드로이드에서 IPC(Inter Process Communication 즉, 프로세스간 커뮤니케이션)는 RPC메커니즘을 근간에 둔 **Binder**가 그 역할을 한다. 

위에서 말한 Activity to Activity나 Activity to Service가 같은 프로세스가 아닌 경우라면 결국 다른 프로세스에 전달되어야 한다. 그래서 Binder에서 여러 정보를 담아 전달할때 Parcel에 담아보내야하고 Parcelable을 구현한 객체만 전달이 가능하다.

## Parcelable

Parcel(소포)라는 이름에서 유추할 수 있듯 데이터들을 묶어놓았다고 생각하면 쉽다. `Parcelable`은 `Serializable`과 달리 override해야 하는 부분이 많다.

```kotlin
class MyParcelable : Parcelable {

    private val mData: Int

    private constructor(input: Parcel) {
        mData = input.readInt()
    }

    override fun describeContents(): Int {
        return 0
    }

    override fun writeToParcel(output: Parcel, flags: Int) {
        output.writeInt(mData)
    }

    companion object {
        val CREATOR: Parcelable.Creator<MyParcelable> = object : Parcelable.Creator<MyParcelable> {
            override fun createFromParcel(source: Parcel): MyParcelable =
                MyParcelable(source)

            override fun newArray(size: Int): Array<MyParcelable?> {
                return arrayOfNulls<MyParcelable>(size)
            }
        }
    }

}
```

최근 `@Parcelize` 어노테이션을 통해서 쉽게 구현이 가능해졌다.

```kotlin
@Parcelize
data class Foo(
    val amount: String,
    val name: Int
) : Parcelable
```

## 만약 자바가 아니라면?

Java 시스템에서 Java 시스템으로의 이동이 아니라면 결국 어떠한 규칙에 의해서 데이터를 담아 전달하고 받고나서도 데이터가 온전히 유지되어야한다. 그래서 우리가 사용하는 것이 익히 알고 있는 JSON, xml, yaml 등 이다. 클라이언트에서 서버로, 서버에서 클라이언트로 주고 받는 경우에 왜 우리가 JSON에 데이터를 담았는지 이해가 되는 부분이다.

## Ref.

[안드로이드 앱 프로세스 분리하기](https://brunch.co.kr/@huewu/4)
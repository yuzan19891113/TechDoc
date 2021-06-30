# Resource Load

## Editor

### General object Load

关键入口：ReadObjectFromPersistentManager

![](../../../.gitbook/assets/image%20%28159%29.png)

### Shader Load

![Shader Load](../../../.gitbook/assets/image%20%28167%29.png)

#### LoadFileCache\(libaray/metaData/\)

1.OpenFile

```text
LocalFileSystemWindowsShared::Open
```

![OpenFile](../../../.gitbook/assets/image%20%28160%29.png)

2.ReadFileCache

```text
SerializedFile::InitializedRead ->m_ReadFile(FileCacherRead)
SerializedFile::ReadHeader
SerializedFile::ReadFileCache
    FileCacherRead::LockCacheBlock
    FileCacherRead::UnLockCacheBlock
SerializedFile::ReadMetaData ->m_Object
```

#### Initialize Object

![](../../../.gitbook/assets/image%20%28157%29.png)

#### Instance ID to File ID

```text
bool Remapper::InstanceIDToSerializedObjectIdentifier(InstanceID instanceID, SerializedObjectIdentifier& identifier)
```

#### File ID get file stream

```text
void SerializedFile::ReadObject(LocalIdentifierInFileType fileID, ObjectCreationMode mode, bool isPersistent, const TypeTree** oldTypeTree, bool* safeLoaded, Object& object)
{
    ObjectMap::iterator iter = m_Object.find(fileID);
    const ObjectInfo& info = iter->second;
    UInt32 byteStart = info.byteStart + m_ReadOffset;
}
```

#### Stream to Object initialization

```text
void SerializedFile::ReadObject(LocalIdentifierInFileType fileID, ObjectCreationMode mode, bool isPersistent, const TypeTree** oldTypeTree, bool* safeLoaded, Object& object)
{
    Object.VirtualRediectTransfer
}
```


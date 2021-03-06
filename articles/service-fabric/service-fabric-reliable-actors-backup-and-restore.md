---
title: Back-ups maken en herstellen van Azure Service Fabric Actors
description: Meer informatie over het implementeren van back-ups en herstel in uw Azure Service Fabric actors.
ms.topic: conceptual
ms.date: 10/29/2018
ms.custom: devx-track-csharp
ms.openlocfilehash: 05dfb7d09e68373f52831a9e5233316df95ee552
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2021
ms.locfileid: "96571331"
---
# <a name="implement-reliable-actors-backup-and-restore"></a>Reliable Actors back-up en herstel implementeren

> [!NOTE]
> Micro soft raadt u aan om [periodieke back-ups en herstel bewerkingen](service-fabric-backuprestoreservice-quickstart-azurecluster.md) te gebruiken voor het configureren van gegevens back-ups van betrouw bare stateful services en reliable actors. 
> 

In het volgende voor beeld bevat een aangepaste actor service een methode voor het maken van back-ups van actor gegevens door gebruik te maken van de externe listener die al aanwezig is in `ActorService` :

```csharp
public interface IMyActorService : IService
{
    Task BackupActorsAsync();
}

class MyActorService : ActorService, IMyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }

    public Task BackupActorsAsync()
    {
        return this.BackupAsync(new BackupDescription(PerformBackupAsync));
    }

    private async Task<bool> PerformBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store the contents of backupInfo.Directory
           return true;
        }
        finally
        {
           Directory.Delete(backupInfo.Directory, recursive: true);
        }
    }
}
```
```Java
public interface MyActorService extends Service
{
    CompletableFuture<?> backupActorsAsync();
}

class MyActorServiceImpl extends ActorService implements MyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<FabricActorService, ActorId, ActorBase> newActor)
    {
       super(context, typeInfo, newActor);
    }

    public CompletableFuture backupActorsAsync()
    {
        return this.backupAsync(new BackupDescription((backupInfo, cancellationToken) -> performBackupAsync(backupInfo, cancellationToken)));
    }

    private CompletableFuture<Boolean> performBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store the contents of backupInfo.Directory
           return true;
        }
        finally
        {
           deleteDirectory(backupInfo.Directory)
        }
    }

    void deleteDirectory(File file) {
        File[] contents = file.listFiles();
        if (contents != null) {
            for (File f : contents) {
               deleteDirectory(f);
             }
        }
        file.delete();
    }
}
```

In dit voor beeld `IMyActorService` is een extern contract dat implementeert `IService` (C#) en `Service` (Java) en wordt vervolgens geïmplementeerd door `MyActorService` . Door dit externe contract toe te voegen, `IMyActorService` zijn methoden nu ook beschikbaar voor een-client door een externe proxy te maken via `ActorServiceProxy` :

```csharp
IMyActorService myActorServiceProxy = ActorServiceProxy.Create<IMyActorService>(
    new Uri("fabric:/MyApp/MyService"), ActorId.CreateRandom());

await myActorServiceProxy.BackupActorsAsync();
```
```Java
MyActorService myActorServiceProxy = ActorServiceProxy.create(MyActorService.class,
    new URI("fabric:/MyApp/MyService"), actorId);

myActorServiceProxy.backupActorsAsync();
```

Lees de volgende artikelen voor meer informatie over Reliable Actors:
* [Beheer van actor status](service-fabric-reliable-actors-state-management.md)
* [Actor-levens cyclus en garbagecollection](service-fabric-reliable-actors-lifecycle.md)
* [Naslag documentatie voor actors-API](/previous-versions/azure/dn971626(v=azure.100))
* [.NET-voorbeeld code](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java-voorbeeld code](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png

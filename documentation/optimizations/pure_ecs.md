---
description: Your entities have become fat and ugly? Let's change that!
---

# PURE\_ECS

We all know it... You let yourself go a bit and then... you weigh too much again and don't know what to do. I've heard that this is also the case for your [**entities**](../entity.md)? I found an Ancient Relic to fix that, check it out!

This is where `PURE_ECS` comes into play. This is a way of reducing the empty size of each [**entity**](../entity.md) to just **4 bytes**! **The way to make your** [**world**](../world.md) **even more efficient!** This means that even more entities fit into each individual chunk, which means that your **queries** **process** the **entities** even **more efficiently**. All entity operations speed up automatically. You want to bring millions of entities into battle? You can!

## A new beginning

How do you do that, you ask? It takes a bit of work, but it's worth it.&#x20;

As this feature is based on **C# preprocessor flags**, you first need the source. You must therefore include [**Arch's source code**](https://github.com/genaray/Arch/tree/master/src/Arch) in your project and cannot use the Arch nugget. You will also need an [**Arch's source generator**](https://github.com/genaray/Arch/tree/master/src/Arch.SourceGen). The best way to do this is to [**fork Arch**](https://github.com/genaray/Arch/fork). Once you've done that, you only have to do one thing. [Set `PURE_ECS` as a flag for the project in the C# project configurations.](https://www.jetbrains.com/help/rider/Build\_Configurations.html#dgy9hp\_14)

Now you have your fork with `PURE_ECS` as preprocessor flag. To use this in your own project, you only need to do one more thing. Remove Arch as nuget and add your own Arch fork to your own project to enjoy the new features.

How you integrate this is up to you. You can copy your Arch fork manually into your project, for example. What I recommend, however, are [**Git submodules**](https://git-scm.com/book/en/v2/Git-Tools-Submodules). This way you can simply integrate your own Arch fork into your project using Git.

## Survival of the fittest

So now you have Arch with `Pure_ECS` in your project... what now? Firstly, you should notice that you can no longer work directly on the [`Entity`](../entity.md) struct. This is intentional, the entity has been minimised and no longer knows which world it is in. You have to adapt and tell it yourself!

```csharp
var gimli = world.Create(new Dwarf(), new Position(), new Velocity());

// Making our dwarf teleport
if(gimli.Has<Position>()){
    ref var pos = ref gimli.Get<Position>();
    gimli.Set(TeleportRandomlyNear(ref pos);
}
```

{% hint style="danger" %}
All methods of the [`Entity`](../entity.md) struct are no longer available with `PURE_ECS`. So you can no longer do this.
{% endhint %}

```csharp
// Making our dwarf teleport
if(world.Has<Position>(gimli)){
    ref var pos = ref world.Get<Position>(gimli);
    world.Set(gimli, TeleportRandomlyNear(ref pos);
}
```

{% hint style="success" %}
Instead, you now have to work entirely with the [`world`](../world.md). And always tell the world on which entity you want to execute what. All methods that were originally available in the [`Entity`](../entity.md) are also accessible in the world.
{% endhint %}

Nothing else changes. Almost a free optimisation... Pretty cool, isn't it?
To start modding with Fabric, we will need an IDE like [Eclipse](https://www.eclipse.org/downloads/) or [IntelliJ IDEA Community Edition.](https://www.jetbrains.com/idea/download/?section=windows)Once you have an IDE started & setup, we will need to setup a Fabric Mod. This can be easily done using the [Fabrics Template Generator.](https://fabricmc.net/develop/template/)The Template will need to be extracted because its a .zip file. Once you have done this, you can open it in your IDE.

After waiting for a short time to load the Project, you will see a few Folders. The Folder we are looking for is the `src` folder. Inside this `src` folder you will find the `client` and `main.`

![[Pasted image 20241002182630.png]]

Inside your package `llama.administration.tatakei.sniffer.jam - as an example` you will find a Java Class named after your `Mod ID`. If you don't like your current `Mod ID`, you are able to change it in the `public static final String MOD_ID = "your_mod_id_here";`


```java
// Your Main Class

package llama.administration.tatakei.sniffer.jam;  
  
import net.fabricmc.api.ModInitializer;  
  
import org.slf4j.Logger;  
import org.slf4j.LoggerFactory;  
  
public class Sniffercraft implements ModInitializer {  
    public static final String MOD_ID = "sniffercraft";  
  
    @Override  
    public void onInitialize() {  
       LOGGER.info("Hello Fabric world!");  
    }}
```

Create a new Java Class inside of your package by right-clicking it.
![[Pasted image 20241002183234.png]]

Name the new Class whatever you like to, in this Instance I will call it `ModItems.` Inside of this `ModItems` Class you are able to `register Items`, create `Items` and create `ItemGroups`.

# Registering Items & Item Groups

## Registering Items

Registering Items is a one-time thing only. Once you have the code done, you don't have to do it again. The code is placed inside of the `public class ModItems` but outside of the `public static void initialize()`

```java
// 7 Line Code, more readable
public static Item register(Item item, String id) {  
       // Create the identifier for the item.  
       Identifier itemID = Identifier.of(Sniffercraft.MOD_ID, id);  
  
       // Return the registered item!  
       return Registry.register(Registries.ITEM, itemID, item);  
   }
```

```java
// 1 Line Code, less readable
public static Item register(Item item, String id) {Identifier itemID = Identifier.of(Sniffercraft.MOD_ID, id); return Registry.register(Registries.ITEM, itemID, item);}
```

Make sure to replace `Sniffercraft` with your actual `MOD_ID`!

## Registering Item Groups

Inside of your `public static void initialize()` method, you register Item Groups and add Items to a Item Group.

```java
public static void initialize() {  
    // Item Group Maker  
    ItemGroupEvents.modifyEntriesEvent(CUSTOM_ITEM_GROUP_KEY).register(itemGroup -> {  
        itemGroup.add(ModItems.SNIFFER_SEEDS); // Adds the Item to the Item Group  
    });  
  
    // Register the group.  
    Registry.register(Registries.ITEM_GROUP, CUSTOM_ITEM_GROUP_KEY, CUSTOM_ITEM_GROUP);
}
```

At the end of your code, outside of your `initialize()` method, you register the Item Group.

```java
public static final RegistryKey<ItemGroup> CUSTOM_ITEM_GROUP_KEY = RegistryKey.of(Registries.ITEM_GROUP.getKey(), Identifier.of(Sniffercraft.MOD_ID, "item_group"));  
    public static final ItemGroup CUSTOM_ITEM_GROUP = FabricItemGroup.builder()  
            .icon(() -> new ItemStack(ModItems.SNIFFER_SEEDS))  
            .displayName(Text.translatable("itemGroup.sniffercraft"))  
            .build();  
}
```

Make sure to replace `Sniffercraft` with your actual `MOD_ID`. In `"itemGroup.sniffercraft"` only replace `sniffercraft` but not with your `MOD_ID` but something else (name of your mod works best)
# Creating a Item

You have registered & made a Item Group, now we can begin creating our own Items. We create Items outside of the `initialize()` method. There is one way of creating an Item but you can have it with two different designs.

```java
// Design 1, 4 Line Code, more Readable
public static final Item TEST_ITEM = register(  
           new Item(new Item.Settings().maxCount(64)),  
           "test_item"  
   );
```

```java
// Design 2, 1 Line Code, less readable
public static final Item TEST_ITEM = register(new Item(new Item.Settings().maxCount(64)), "test_item");
```

Don't forget to add the Item to your Item Group or else it won't show up.
```java
public static void initialize() {  
    // Item Group Maker  
    ItemGroupEvents.modifyEntriesEvent(CUSTOM_ITEM_GROUP_KEY).register(itemGroup -> {  
        itemGroup.add(ModItems.TEST_ITEM); // Adds the Item to the Item Group  
    });  
  
    // Register the group.  
    Registry.register(Registries.ITEM_GROUP, CUSTOM_ITEM_GROUP_KEY, CUSTOM_ITEM_GROUP);
}
```
# Creating a Food Item

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

To make our first Food Item, we need to create a `FoodComponent` and register the Item. While making our `FoodComponent` we are able to give it different `AttributeModifiers`. There are 6 different `AttributeModifiers` for a `FoodComponent`.

### nutrition()

Inside of the `FoodComponent builder` we are able to set the nutrition the food gives upon eating. `Nutrition(1)` is half a hunger bar and goes up to `nutrition(20)`.

```java
public static final FoodComponent SNIFFER_SEEDS_FOOD_COMPONENT = new FoodComponent.Builder()
        .nutrition(10) // In this case, half of your hunger bar would be filled.
        .build();
```
### saturationModifier()

We are able to set how much saturation the player gets upon eating.

```java
public static final FoodComponent SNIFFER_SEEDS_FOOD_COMPONENT = new FoodComponent.Builder()
		.nutrition(10)
		.saturationModifier(5) // Unknown at the moment
		.build();
```
### meat()

`meat()` sets if your food is able to be eaten by wolfs or not.

```java
public static final FoodComponent SNIFFER_SEEDS_FOOD_COMPONENT = new FoodComponent.Builder()
		.nutrition(10)
		.saturationModifier(5)
		.meat() // Lets wolfs eat your food
		.build();
```
### alwaysEdible()

`alwaysEdible()` makes the player being able to eat it no matter if their hunger bar is full. Take the golden apple as an example.

```java
public static final FoodComponent SNIFFER_SEEDS_FOOD_COMPONENT = new FoodComponent.Builder()
		.nutrition(10)
		.saturationModifier(5)
		.meat()
		.alwaysEdible()
		.build();
```

### snack()

`snack()` makes your food fast to eat, take dried kelp as an example.

```java
public static final FoodComponent SNIFFER_SEEDS_FOOD_COMPONENT = new FoodComponent.Builder()
		.nutrition(10)
		.saturationModifier(5)
		.meat()
		.alwaysEdible()
		.snack()
		.build();
```
### statusEffect()

`statusEffect()` gives the user a statusEffect upon eating the Food. Golden Apples and Enchanted Golden Apples are a good example as they give the user effects.

```java
public static final FoodComponent SNIFFER_SEEDS_FOOD_COMPONENT = new FoodComponent.Builder()
		.nutrition(10)
		.saturationModifier(5)
		.meat()
		.alwaysEdible()
		.snack()
		.statusEffect(new StatusEffectInstance(StatusEffects.RESISTANCE, 6 * 20, 1), 1.0f) // Duration is in ticks, 20 ticks = 1 second, 6 * 20t = 120 ticks
		.build();
```
# Registering a Food Component

Once you have chosen the `attributeModifiers` you like, you have to register the `FoodComponent.` Its similar to registering a normal item, but not the same. The item always gets registered after the `FoodComponent` is created.

```java
public static final FoodComponent SNIFFER_SEEDS_FOOD_COMPONENT = new FoodComponent.Builder()
		.nutrition(10)
		.build()

public static final Item SNIFFER_SEEDS = register(  
        new Item(new Item.Settings().food(SNIFFER_SEEDS_FOOD_COMPONENT).maxCount(64)),  
        "sniffer_seeds"  
);
```
# Custom Item Interactions
# Custom Armor
# Custom Tools

Making Tools isn't easy and is quite confusing at first. To make our own tools, we need `Enum Java Class` named after our custom Material, in this example it is called `SnifferMaterial`.

Inside of this `SnifferMaterial class` we can set our own `itemDurability, miningSpeed, attackDamage, enchantability` and the `repairIngredient`.

```java
// SnifferMaterial Class Code Block
package llama.administration.tatakei.sniffer.jam.tools;  
  
import llama.administration.tatakei.sniffer.jam.ModItems;  
import net.minecraft.block.Block;  
import net.minecraft.item.ToolMaterial;  
import net.minecraft.recipe.Ingredient;  
import net.minecraft.registry.tag.BlockTags;  
import net.minecraft.registry.tag.TagKey;  
  
import java.util.function.Supplier;  
  
public enum SnifferMaterial implements ToolMaterial {  
    SNIFFER(BlockTags.INCORRECT_FOR_NETHERITE_TOOL, 1800, 18.0F, 0.0F, 22, () -> Ingredient.ofItems(ModItems.SNIFFER_SEEDS));  
  
    private final TagKey<Block> inverseTag;  
    private final int itemDurability;  
    private final float miningSpeed;  
    private final float attackDamage;  
    private final int enchantability;  
    private final Supplier<Ingredient> repairIngredient;  
  
    SnifferMaterial(TagKey<Block> inverseTag, int itemDurability, float miningSpeed, float attackDamage, int enchantability, Supplier<Ingredient> repairIngredient) {  
        this.inverseTag = inverseTag;  
        this.itemDurability = itemDurability;  
        this.miningSpeed = miningSpeed;  
        this.attackDamage = attackDamage;  
        this.enchantability = enchantability;  
        this.repairIngredient = repairIngredient;  
    }  
  
    @Override  
    public int getDurability() {  
        return this.itemDurability;  
    }  
    @Override  
    public float getMiningSpeedMultiplier() {  
        return this.miningSpeed;  
    }  
    @Override  
    public float getAttackDamage() {  
        return this.attackDamage;  
    }  
    @Override  
    public TagKey<Block> getInverseTag() {  
        return this.inverseTag;  
    }  
    @Override  
    public int getEnchantability() {  
        return this.enchantability;  
    }  
    @Override  
    public Ingredient getRepairIngredient() {  
        return this.repairIngredient.get();  
    }}
```

In this example `SnifferMaterial class` we have: 
```
itemDurability: 1800
miningSpeed: 18.0F
attackDamage: 0.0F
enchantability: 22
repairIngredient: SNIFFER_SEEDS
```
These are just the base stats the tool will have. Inside of our `ModItems class` we have to create & register the Tool Item. Outside of the `initialize()` method, we create the Tool Item.

```java
public static final Item SNIFFER_PICKAXE = registerItem("sniffer_pickaxe",  
        new PickaxeItem(SnifferMaterial.SNIFFER, new Item.Settings().attributeModifiers(PickaxeItem.createAttributeModifiers(SnifferMaterial.SNIFFER, 2, -2.4F))));
```

`registerItem` is showing as a error because it hasn't been registered yet.
This will create a `Sniffer Pickaxe` with a `baseAttackDamage` of 2, and a `attackSpeed` of -2.4F. A `attackSpeed` of `-4.0F` would be slow compared to `-0.0F` which would be fast. Depending on which Tool you are creating, you need to change `new PickaxeItem` to something else.

```
Pickaxe: new PickaxeItem
Axe: new AxeItem
Sword: new SwordItem
Shovel: new ShovelItem
Hoe: new HoeItem
```

`registerItem` is showing as an error because we haven't made the `registerItem` method yet. After you created your Tool Item, we have to register the Item. To avoid any errors, make sure you change the `MOD_ID` to your own `MOD_ID`!

```java
private static Item registerItem(String name, Item item) {  
    return Registry.register(Registries.ITEM, Identifier.tryParse(Sniffercraft.MOD_ID + ":" + name), item);  
}
```
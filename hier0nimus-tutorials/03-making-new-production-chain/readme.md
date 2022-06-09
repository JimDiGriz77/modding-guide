# Tutorial 3 - Making a new production chain

For our next mod we are taking another step in a more advanced mod. We are going to make a new production chain and a new good. We are not going to make it that complex for now and start with a basic production chain and one new product. We are going to make **citrus tea**.

## What are we going to do?

We are going to use an already existing good (**citrus**) to create a new good (**Citrus Tea**).

We will be creating:

- Product (Citrus Tea)
- Production chain (Citrus Tea production chain)
- Citizen needs (Citrus Tea as a Luxury Need for Engineers and Investors)
- Building texture. No 3D-modelling yet, just the texture.
- Icons
- Specialist

This can be a big step because it has a lot of small steps we need to take. But do not worry, we are going to do this step by step.

## Our mod structure

Compared to the previous mod we will now have some more folders and files we need to create. We will call our mod "Tutorial Specialists" The structure now will look like this:

- [Gameplay] Tutorial Citrus Tea
  - data
    - config
      - export
        - main
          - asset
            - xml file (assets.xml) which contains all the logic for our mod.
      - gui
        - xml files containing the translations for text in every language
          - texts_english.xml
          - texts_french.xml
          - texts_german.xml
          - …
    - graphics
      - buildings
        - production
          - production_citrus_tea  
            Contains production_citrus_tea files that describes the new building. - maps  
             Contains maps which are the materials/textures of our building. diff, mask, metal and norm file.
      - icons
        - Custom graphics we use in our mod. Icon for Citrus Tea and icon for specialist.

## Start with the icons

The icons we need for this mod are:

- Citrus Tea good
- Specialist

### Citrus Tea

If we look at what is already available in the game we have the Hibiscus Tea and citrus. Both icons can be recycled to make a new Citrus Tea icon.

For the Citrus icon we go to data18.rda/data/ui/2kimages/main/3dicons/icon_citrus_0.dds and convert the dds to a png.

For the Hibiscus Tea icon we go to data16.rda/data/ui/2kimages/main/3dicons/goods_africa/icon_hibiscus_tea_0.dds and convert the dds to a png.

![icon_citrus_0.png](./_sources/icon_citrus_0.png)
![icon_hibiscus_tea_0.png](./_sources/icon_hibiscus_tea_0.png)

We do some magic in our photo editing program and voila, we have a nice result.

![citrus-tea.png](./_sources/citrus-tea.png)

## Specialist

For the specialist, let's take a man and name him Tony Lipus, Citrua tea smasher. We go to the data2.rda/data/ui/2kimages/main/3dicons/specialists/systemic/icon_farmer_106_0.dds and convert this dds to a png.

![icon_farmer_106_0.png](./_sources/icon_farmer_106_0.png)

We do again some editing magic to get the result below. Changing some colors.

![icon_Tony Lipus_v2.png](./_sources/icon_tony_lipus_v2.png)

So, we now have our icons and can start with the next step.

## Our GUIDS

Let's start with defining all the GUIDS we need for this mod.

- 1742008803 (Tony Lipus, Citrus tea smasher)
- 1742008804 (Tony Lipus description)
- 1742008805 (Citrus Tea Good)
- 1742008806 (Citrus Tea Good description)
- 1742008807 (Citrus Tea Chain)
- 1742008808 (Citrus Tea Chain description)
- 1742008809 (Citrus Tea Dryer Building)
- 1742008810 (Citrus Tea Dryer Building description)
- 1742008811 (Trigger unhide)
- 1742008812 (Trigger unlock)

## Tony Lipus, Citrus tea smasher

Let's start with our specialist. We know from the previous tutorial how to create a new specialist. So that should not be that hard. See this as an extra exercise.

Steps we need to take:

- Create the translations
- Create the specialist with his proporties
- Add to rewardpool
- Add to trigger

### Create translation

Create the translation for our name of our specialist and the description.

```XML
<ModOps>
  <ModOp Type="add" Path="/TextExport/Texts">
    <!-- START SPECIALIST - Tony Lipus, Citrus tea smasher -->
    <Text>
      <GUID>1742008803</GUID>
      <Text>Tony Lipus, Citrus tea smasher</Text>
    </Text>
    <Text>
      <GUID>1742008804</GUID>
      <Text>Tony does not like the hibiscus tea. He prefers citrus fruits and made sure his tea is better then the hibiscus tea. He bottles some extra lemonade without any additive sugar. Sugar is the new smoking!</Text>
    </Text>
    <!-- END SPECIALIST - Tony Lipus, Citrus tea smasher -->
</ModOps>
```

### Create our specialist

We already created his **name with main GUID** (GUID 1742008803) and **description** (GUID 1742008804). The icon is also already created and we know the path to the icon.

We need to define what we want to do with the specialist. Tony is a specialist for the building that processes **Citrus** to **Citrus Tea**, the **Citrus Tea Dryer** (GUID 1742008809).

Let's make it a **Legendary Specialist** (Rarity & ItemType) item for the **Guildhouse** (Template & Allocation), because it is a specialist for a production building.

A **production boost** is always good (ProductivityUpgrade), so let's do that already for 60%.

In the description we talk about Tony hating additive sugar in **Lemonade** (GUID 133185) and making sugar free lemonade. This can be an extra good he produces when producing Citrus Tea. Let's say **every 4 cycles**.

The building he will have effect on (ItemEffect) is one of the buildings we still need to create, **Citrus Tea Dryer** (GUID 1742008809).

For **Expeditions** (ExpeditionAttribute), he is **male** (PerkMale) and is good at **Medicine - 50 and Diplomacy - 15**.

Let's also give him a **reduction for the workforce of 30%** (BuildingUpgrade).

```XML
  <!-- START SPECIALIST - Tony Lipus, Citrus tea smasher -->
  <ModOp GUID="191388" Type="addNextSibling">
    <Asset>
      <Template>GuildhouseItem</Template>
      <Values>
        <Standard>
          <GUID>1742008803</GUID> <!-- SPECIALIST - Tony Lipus, Citrus tea smasher -->
          <Name>Tony Lipus, Citrus tea smasher</Name>
          <IconFilename>data/modgraphics/icons/icon_tony_lipus_v2.png</IconFilename>
          <InfoDescription>1742008804</InfoDescription>
        </Standard>
        <Text>
          <LocaText>
            <English>
              <Text>Item Template</Text>
              <Status>Exported</Status>
              <ExportCount>1</ExportCount>
            </English>
          </LocaText>
        </Text>
        <Locked />
        <Buff />
        <Item>
          <MaxStackSize>1</MaxStackSize>
          <Rarity>Legendary</Rarity>
          <ItemType>Specialist</ItemType>
          <Allocation>GuildHouse</Allocation>
          <TradePrice>1250000</TradePrice>
          <TradePriceOnlineCurrency>1250000</TradePriceOnlineCurrency>
        </Item>
        <FactoryUpgrade>
          <ProductivityUpgrade>
            <Value>60</Value>
            <Percental>1</Percental>
          </ProductivityUpgrade>
          <AdditionalOutput>
            <Item>
              <Product>133185</Product> <!-- Lemonade -->
              <AdditionalOutputCycle>4</AdditionalOutputCycle>
              <Amount>1</Amount>
            </Item>
          </AdditionalOutput>
        </FactoryUpgrade>
        <ItemEffect>
          <EffectTargets>
            <Item>
              <GUID>1742008809</GUID> <!-- Citrus Tea Dryer -->
            </Item>
          </EffectTargets>
        </ItemEffect>
        <ItemAction />
        <ExpeditionAttribute>
          <BaseMorale>0</BaseMorale>
          <ExpeditionAttributes>
            <Item>
              <Attribute>PerkMale</Attribute>
              <Amount>1</Amount>
            </Item>
            <Item>
              <Attribute>Medicine</Attribute>
              <Amount>50</Amount>
            </Item>
            <Item>
              <Attribute>Diplomacy</Attribute>
              <Amount>15</Amount>
            </Item>
            <Item />
            <Item />
          </ExpeditionAttributes>
          <ItemDifficulties>Hard</ItemDifficulties>
        </ExpeditionAttribute>
        <BuildingUpgrade>
          <WorkforceAmountUpgrade>
            <Value>-30</Value>
            <Percental>1</Percental>
          </WorkforceAmountUpgrade>
        </BuildingUpgrade>
      </Values>
    </Asset>
  </ModOp>
  <!-- END SPECIALIST - Tony Lipus, Citrus tea smasher -->
```

### Add to rewardpool

Adding the specialist to some rewardpools should not be that hard anymore. Pick some you like maybe? We take the following for now:

- 192840 - Specialists - Europe and SA Goods - Legendary
- 193963 - Specialists - Europe and SA Goods - Legendary - Guild
- 193079 - Specialists - Investor Specialists - Legendary

```XML
  <!-- START  ADD TO REWARDPOOL - Tony Lipus, Citrus tea smasher -->
  <ModOp GUID="192840,193963,193079" Type="add" Path="/Values/RewardPool/ItemsPool">
    <Item>
      <ItemLink>1742008803</ItemLink> <!-- SPECIALIST - Tony Lipus, Citrus tea smasher -->
    </Item>
  </ModOp>
  <!-- END ADD TO REWARDPOOL - Tony Lipus, Citrus tea smasher -->
```

### Add to trigger

We will already create our main triggers that we will use for the other assets of this mod.

We know how the triggers work. We just need to define when to trigger the trigger. In our case, we need citrus to be available to make citrus tea. Citrus is one of the Tourist goods that unlocks at a certain amount of tourists. So we can only unlock Citrus Tea when we can actually also make citrus. We unhide everything for Citrus Tea at 750 tourists and unlock everything at 1250 tourists. In this case we do have 2 separate triggers. One for unhide and one for unlock

```XML
  <!-- START TRIGGER -->
  <ModOp Type="addnextSibling" GUID="130248">
    <!--UNHIDE - 750 Tourists -->
    <Asset>
      <Template>Trigger</Template>
      <Values>
        <Standard>
          <GUID>1742008811</GUID>
          <Name>MOD Trigger</Name>
        </Standard>
        <Trigger>
          <TriggerCondition>
            <Template>ConditionPlayerCounter</Template>
            <Values>
              <Condition />
              <ConditionPlayerCounter>
                <PlayerCounter>PopulationByLevel</PlayerCounter>
                <Context>601379</Context> <!-- Tourists -->
                <CounterAmount>750</CounterAmount>
              </ConditionPlayerCounter>
            </Values>
          </TriggerCondition>
          <TriggerActions>
            <Item>
              <TriggerAction>
                <Template>ActionUnlockAsset</Template>
                <Values>
                  <Action />
                  <ActionUnlockAsset>
                    <UnhideAssets>
                      <Item>
                        <Asset>1742000200</Asset> <!-- SPECIALIST - Tony Lipus, Citrus tea smasher -->
                      </Item>
                      </Item>
                    </UnhideAssets>
                  </ActionUnlockAsset>
                </Values>
              </TriggerAction>
            </Item>
          </TriggerActions>
        </Trigger>
        <TriggerSetup />
      </Values>
    </Asset>
  </ModOp>
  <ModOp Type="addnextSibling" GUID="130248">
    <!--UNLOCK - 1250 Tourists -->
    <Asset>
      <Template>Trigger</Template>
      <Values>
        <Standard>
          <GUID>1742008812</GUID>
          <Name>MOD Trigger</Name>
        </Standard>
        <Trigger>
          <TriggerCondition>
            <Template>ConditionPlayerCounter</Template>
            <Values>
              <Condition />
              <ConditionPlayerCounter>
                <PlayerCounter>PopulationByLevel</PlayerCounter>
                <Context>601379</Context> <!-- Tourists -->
                <CounterAmount>1250</CounterAmount>
              </ConditionPlayerCounter>
            </Values>
          </TriggerCondition>
          <TriggerActions>
            <Item>
              <TriggerAction>
                <Template>ActionUnlockAsset</Template>
                <Values>
                  <Action />
                  <ActionUnlockAsset>
                    <UnlockAssets>
                      <Item>
                        <Asset>1742000200</Asset> <!-- SPECIALIST - Tony Lipus, Citrus tea smasher -->
                      </Item>
                      </Item>
                    </UnlockAssets>
                  </ActionUnlockAsset>
                </Values>
              </TriggerAction>
            </Item>
          </TriggerActions>
        </Trigger>
        <TriggerSetup />
      </Values>
    </Asset>
  </ModOp>
  <!-- END TRIGGER -->
```

Perfect! We now have our specialist ready to use!

## Citrus Tea, the new good

Time to create our new good. We already have the icon for it. The rest is also not that complicated. A good has not that many proporties.

### addNextSibling, Template, GUID, name, description, text

We will be adding this new good to the list of goods right after for example fish (GUID 1010200)

The **template** we use for a new good is &lt;Template>Product&lt;/Template>.

```XML
  <!-- START ADD GOOD - Citrus Tea -->
  <ModOp Type="addNextSibling" GUID='1010200'>
    <Asset>
      <Template>Product</Template>
    </Asset>
  </ModOp>
  <!-- END ADD GOOD - Citrus Tea -->
```

We add the GUID, name, icon and GUID of the description.

```XML
  <!-- START ADD GOOD - Citrus Tea -->
  <ModOp Type="addNextSibling" GUID='1010200'>
    <Asset>
      <Template>Product</Template>
      <Values>
        <Standard>
          <GUID>1742008805</GUID>
          <Name>Citrus Tea</Name>
          <IconFilename>data/modgraphics/icons/citrus-tea.png</IconFilename>
          <InfoDescription>1742008806</InfoDescription>
        </Standard>
      </Values>
    </Asset>
  </ModOp>
  <!-- END ADD GOOD - Citrus Tea -->
```

Then we add the fallback text.

```XML
  <!-- START ADD GOOD - Citrus Tea -->
  <ModOp Type="addNextSibling" GUID='1010200'>
    <Asset>
      <Template>Product</Template>
      <Values>
        <Standard>
          <GUID>1742008805</GUID>
          <Name>Citrus Tea</Name>
          <IconFilename>data/modgraphics/icons/citrus-tea.png</IconFilename>
          <InfoDescription>1742008806</InfoDescription>
        </Standard>
        <Text>
          <LocaText>
            <English>
              <Text>Citrus Tea</Text>
              <Status>Exported</Status>
              <ExportCount>1</ExportCount>
            </English>
          </LocaText>
        </Text>
      </Values>
    </Asset>
  </ModOp>
  <!-- END ADD GOOD - Citrus Tea -->
```

### Product

Next is the important part of the product. It defines the most important proporties of the product.

#### StorageLevel

This can be:

- Building
- Area

Building is used for alle production goods.

Area is used for all "workforce products".

In our case it is a production good so, **building**.

#### ProductCategory

Products are categorised. We have different categories:

- 11702 - Product Category Resource
- 11703 - Product Category Intermediate Good
- 11704 - Product Category Supply Good
- 11705 - Product Category Happiness Good
- 11706 - Product Category Money Good
- 11707 - Product Category Construction Material
- 11708 - Product Category Public Supply
- 11709 - Product Category Public Happiness
- 11710 - Product Category Public Money
- 11710 - Product Category Public Money
- 134777 - Product Category Bus Need
- 135875 - Product Category Commercial Supply

In our case, it is the Product Category Happiness Good (GUID 11705).

#### BasePrice

We define a basprice for our good.

#### CivLevel

This will define on which citizen level the good is used. In our case it will be used for Engineers and Investors. So, CivLevel 4.

#### AssociatedRegion

Define where this good is assosiated with. There are a couple of regions we can choose from:

- Moderate (Old World)
- Colony01 (New World)
- Arctic (Arctic)
- Africa (Enbesa)

We can combine multiple regions. For example for us the citrus comes from the New World and we produce the good in the Old World. So both are associated with our good.

#### ProductionRegions

##### RegionType

Define where this good will be produced. There are a couple of regions we can choose from:

- Moderate (Old World)
- Colony01 (New World)
- Arctic (Arctic)
- Africa (Enbesa)

We can also combine multiple regions. But we only need 1 in this case. It will only be produced in the Old World.

##### RequiredDLCs

Lock the good if the required DLC is not available. We already saw this list before but here it is again:

- 410003 (Imperial Pack)
- 410021 (Season Pass Player Assets)
- 410069 (Season 2 Pass Reward)
- 305 (Season 3 Pass Reward)
- 25945 (Season 4 Bonus Content)
- 410079 (Amusements Pack)
- 116630 (Holiday Ornament Pack)
- 4100010 (The Anarchist)
- 410040 (Sunken Treasures)
- 410041 (Botanica)
- 410042 (The Passage)
- 410059 (Seat Of Power)
- 410070 (Bright Harvest)
- 410071 (Land of Lions)
- 410083 (Docklands)
- 410084 (Tourist Season)
- 410085 (The High Life)
- 24961 (Seeds Of Change)

We need in our case the Tourist DLC (GUID 410084).

```XML

<Product>
    <StorageLevel>Building</StorageLevel>
    <ProductCategory>11705</ProductCategory> <!-- Happiness Good -->
    <BasePrice>150</BasePrice>
    <CivLevel>4</CivLevel>
    <AssociatedRegion>Moderate;Colony01</AssociatedRegion>
    <ProductionRegions>
        <Item>
            <RegionType>Moderate</RegionType>
            <RequiredDLCs>
                <Item>
                    <RequiredDLC>410084</RequiredDLC> <!-- Tourist Season -->
                </Item>
            </RequiredDLCs>
        </Item>
    </ProductionRegions>
</Product>

```

### ExpeditionAttribute

Define a base flat amount for morale and a flat amount if you want to add a specific attribute to the good.

We already saw the list but here it is again:

- Crafting
- Diplomacy
- Melee (Force)
- Might (Naval Power)
- Navigation
- Medicine
- Faith
- Hunting

In our case we add 0 base morale but an extra attribute of 20 medicine.

```XML
<ExpeditionAttribute>
    <BaseMorale>0</BaseMorale>
    <ExpeditionAttributes>
        <Item>
            <Attribute>Medicine</Attribute>
            <Amount>20</Amount>
        </Item>
    </ExpeditionAttributes>
</ExpeditionAttribute>
```

If we combine all this we get:

```XML
  <!-- START ADD GOOD - Citrus Tea -->
  <ModOp Type="addNextSibling" GUID='1010200'>
    <Asset>
      <Template>Product</Template>
      <Values>
        <Standard>
          <GUID>1742008805</GUID>
          <Name>Citrus Tea</Name>
          <IconFilename>data/modgraphics/icons/citrus-tea.png</IconFilename>
          <InfoDescription>1742008806</InfoDescription>
        </Standard>
        <Text>
          <LocaText>
            <English>
              <Text>Citrus Tea</Text>
              <Status>Exported</Status>
              <ExportCount>1</ExportCount>
            </English>
          </LocaText>
        </Text>
        <Product>
          <StorageLevel>Building</StorageLevel>
          <ProductCategory>11705</ProductCategory> <!-- Luxury Need -->
          <BasePrice>150</BasePrice>
          <CivLevel>4</CivLevel>
          <AssociatedRegion>Moderate</AssociatedRegion>
          <ProductionRegions>
            <Item>
              <RegionType>Moderate</RegionType>
              <RequiredDLCs>
                <Item>
                  <RequiredDLC>410084</RequiredDLC> <!-- Tourist Season -->
                </Item>
              </RequiredDLCs>
            </Item>
          </ProductionRegions>
        </Product>
        <ExpeditionAttribute>
          <BaseMorale>20</BaseMorale>
          <ExpeditionAttributes>
            <Item>
              <Attribute>Medicine</Attribute>
              <Amount>20</Amount>
            </Item>
          </ExpeditionAttributes>
        </ExpeditionAttribute>
      </Values>
    </Asset>
  </ModOp>
  <!-- END ADD GOOD - Citrus Tea -->
```

## Adding the good to ProductStorageLists & ProductList

We have created the good, but the next step in the process is telling the game to which **ProductStorageList** and **ProductList** we want to add the good. If we go to the warehouse or harbour, the goods are divided in different categories. We have to add our good to all the lists we want.

There are a lot of **ProductStorageList** and **ProductList** available, and it is hard in the beginning to find the right lists you need. The only advice I can give in the beginning is to find the first list in the assets.xml and then go through all of them. Then add every list you encounter and think your product can fit in.

### ProductStorageList

To start we will add our product to the ProductStorageLists. We can now go through the whole assets.xml and search for all &lt;Template>ProductStorageList&lt;/Template> and take the ones we need and add our product to those lists also. There are 8 lists in total and 2 of them are the right fit for this good. 120055 (StandardProductStorageList) and 120057 (StandardMarketplaceModerateStorageList).

- 120055 - StandardProductStorageList
- 120056 - StandardOilHarborStorageList
- 120061 - StandardCoalStoneStorageList
- 120057 - StandardMarketplaceModerateStorageList
- 120058 - StandardMarketplaceColonyStorageList
- 120059 - StandardMarketplaceArcticStorageList
- 122362 - StandardMarketplaceAfricaStorageList
- 120060 - EmptyStorageList

We go and look in the assets.xml for this list to be able to know what the path is we need to do the right **Type="add"**.

```XML
<Asset>
  <Template>ProductStorageList</Template>
  <Values>
      <Standard>
          <GUID>120055</GUID>
          <Name>StandardProductStorageList</Name>
      </Standard>
      <ProductStorageList>
          <ProductList>
              <Item>
                  <Product>120008</Product>
              </Item>
              <Item>
                  <Product>1010196</Product>
              </Item>
              <Item>
                  <Product>1010200</Product>
              </Item>
              <Item>
                  <Product>1010195</Product>
              </Item>
              ...
          </ProductList>
      </ProductStorageList>
  </Values>
</Asset>
```

So, if we look at the path we should:

```XML
  <!-- START ADD GOOD TO StandartProductStorageList -->
  <ModOp Type="add" GUID='120055,120057' Path="/Values/ProductStorageList/ProductList">
    <Item>
      <Product>1742008805</Product> <!-- GOOD - Citrus Tea  -->
    </Item>
  </ModOp>
  <!-- END ADD GOOD TO StandartProductStorageList -->
```

### ProductList

We can now go through the whole assets.xml and search for all &lt;Template>ProductList&lt;/Template> and take the ones we need and add our product to those lists. This amount of ProductList is a lot longer. There are at the moment of writing this tutorial 102 ProductLists.

For easy access I added them underneath and put them together based on their function:

Main tab with all products in the kontor.

- 502017 - FarmerUnlockGoods
- 502018 - WorkerUnlockGoods
- 502019 - ArtisanUnlockGoods
- 502020 - EngineerUnlockGoods
- 502066 - EngineerUnlockGoods_WithoutOil
- 502308 - TouristUnlockGoods
- 502021 - InvestorUnlockGoods
- 502134 - ScholarUnlockGoods
- 502022 - JornaleroUnlockGoods
- 502023 - ObreroUnlockGoods
- 502024 - ExplorerUnlockGoods
- 502025 - TechnicianUnlockGoods
- 502219 - TechnicianUnlockGoods_WithoutScrap
- 502071 - ShepherdUnlockGoods
- 502072 - ElderUnlockGoods
- 502175 - ScrapForInKontorFilter

For the idividual tabs (resources, materials,...):

- 501995 - FarmerConsumerGoods
- 501996 - WorkerConsumerGoods
- 501997 - ArtisansConsumerGoods
- 501998 - EngineerConsumerGoods
- 501999 - InvestorConsumerGoods
- 502000 - JornaleroConsumerGoods
- 502001 - ObreroConsumerGoods
- 502002 - ExplorerConsumerGoods
- 502003 - TechnicianConsumerGoods
- 502073 - ShepherdConsumerGoods
- 502074 - ElderConsumerGoods
- 502133 - ScholarConsumerGoods
- 502309 - TouristConsumerGoods
- 501957 - ConstructionMaterials
- 502168 - ConstructionMaterials_AfricaSort
- 502221 - ConstructionMaterials_WithoutScrap
- 502049 - RawMaterials_Europe
- 502050 - RawMaterials_SouthAmerica
- 502051 - RawMaterials_Arctic
- 502075 - RawMaterials_Africa
- 502037 - AgriculturalGoods_Europe
- 502038 - AgriculturalGoods_SouthAmerica
- 502039 - AgriculturalGoods_Arctic
- 502076 - AgriculturalGoods_Africa
- 502043 - IntermediateGoods_Europe
- 502044 - IntermediateGoods_SouthAmerica
- 502045 - IntermediateGoods_Arctic
- 502077 - IntermediateGoods_Africa
- 502031 - AllConsumerGoods_EuropeSort
- 502034 - AllConsumerGoods_SouthAmericaSort
- 502035 - AllConsumerGoods_ArcticSort
- 502081 - AllConsumerGoods_AfricaSort
- 502052 - AllRawMaterials_EuropeSort
- 502053 - AllRawMaterials_SouthAmericaSort
- 502054 - AllRawMaterials_ArcticSort
- 502082 - AllRawMaterials_AfricaSort
- 502040 - AllAgricultureGoods_EuropeSort
- 502041 - AllAgricultureGoods_SouthAmericaSort
- 502042 - AllAgricultureGoods_ArcticSort
- 502083 - AllAgricultureGoods_AfricaSort
- 502046 - AllIntermediateGoods_EuropeSort
- 502047 - AllIntermediateGoods_SouthAmericaSort
- 502048 - AllIntermediateGoods_ArcticSort
- 502084 - AllIntermediateGoods_AfricaSort
- 501955 - AllGoodsWithoutStrategicGoods
- 501961 - StrategicGoods
- 501992 - LuxuryGoods
- 501993 - BasicNeedGoods
- 501994 - HeatGoods

For the workforce boost menu. Position matters!

- 502004 - FarmerWorkforceGoods
- 502005 - WorkerWorkforceGoods
- 502006 - ArtisanWorkforceGoods
- 502007 - EngineerWorkforceGoods
- 502008 - JornaleroWorkforceGoods
- 502009 - ObreroWorkforceGoods
- 502010 - ExplorerWorkforceGoods
- 502011 - TechnicianWorkforceGoods
- 502069 - ShepherdWorkforceGoods
- 502070 - ElderWorkforceGoods

Session goods:

- 502012 - ModerateSessionGoods
- 502013 - SouthAmericaSessionGoods
- 502014 - ArcticSessionGoods
- 502085 - AfricaSessionGoods

Docklands importers. Position matters!

- 502233 - Tattershire_ImportGoods
- 502234 - Feedl_ImportGoods
- 502235 - Chanteuse_ImportGoods
- 502236 - Qinsa_ImportGoods
- 502237 - OldLevants_ImportGoods
- 502238 - Kitea_ImportGoods
- 502239 - PromistTrust_ImportGoods
- 502240 - Ganymedia_ImportGoods
- 502242 - Docklands_ImporterGoods

Not sure what those do:

- 502016 - AllGoodsUnlock_EuropeSort
- 502065 - AllGoodsUnlock_EuropeSort_WithoutOil
- 502027 - AllGoodsUnlock_SouthAmericaSort
- 502067 - AllGoodsUnlock_SouthAmericaSort_WithoutOil
- 502028 - AllGoodsUnlock_ArcticSort
- 502068 - AllGoodsUnlock_ArcticSort_WithoutOil
- 502078 - AllGoodsUnlock_AfricaSort
- 502080 - AllGoodsUnlock_AfricaSort_WithoutOil
- 502220 - AllGoodsUnlock_EuropeSort_WithoutOil_WithoutScrap

Not used anymore:

- 501954 - AllGoods_OLD
- 501956 - ConsumerGoods_OLD
- 501958 - RawMaterials_OLD
- 501959 - AgriculturalGoods_OLD
- 501960 - IntermediateGoods_OLD

Which ones are we going to use? Well, let's go through the whole list.
Important to know is that the order we put the good into the list is important. If we would just use Typ="add" it will be added at the end of the list. If we want to add it immediatly after a certain good we use **Type="addNextSibling"** or the one before an item **Type="addPrevSibling"**.

Our good Citrus Tea is a luxury consumer good available for Engineers and Investors. It is produced by Workers in the Old World. It is an Old World session good. We will make it availble for selling in Docklands by one of the sellers (Old Levant & Co.).

So we are using:

- 502020 - EngineerUnlockGoods
- 502066 - EngineerUnlockGoods_WithoutOil
- 501998 - EngineerConsumerGoods
- 502031 - AllConsumerGoods_EuropeSort
- 501992 - LuxuryGoods
- 502005 - WorkerWorkforceGoods
- 502012 - ModerateSessionGoods
- 502237 - OldLevants_ImportGoods
- 502242 - Docklands_ImporterGoods

We combine some of them to make sure we can add them right after the same products in the lists.

```XML
  <!-- START ADD GOOD TO ProductLists - after Pocket Watches (1010246) -->
  <ModOp Type="addNextSibling" GUID="502020,502066,501998,502031,501992,502012" Path="/Values/ProductList/List/Item[Product='1010246']">
    <Item>
      <Product>1742008805</Product> <!-- GOOD - Citrus Tea  -->
    </Item>
  </ModOp>
  <!-- END ADD GOOD TO ProductList -->

  <!-- START ADD GOOD TO WorkerWorkforceGoods - after bread (1010213) -->
  <ModOp Type="addNextSibling" GUID="502005" Path="/Values/ProductList/List/Item[Product='1010213']">
    <Item>
      <Product>1742008805</Product> <!-- GOOD - Citrus Tea  -->
    </Item>
  </ModOp>
  <!-- END ADD GOOD TO WorkerWorkforceGoods -->

  <!-- START ADD GOOD TO OldLevants_ImportGoods + Docklands_ImporterGoods - after Hibiscus Tea (114390) -->
  <ModOp Type="addNextSibling" GUID="502005" Path="/Values/ProductList/List/Item[Product='114390']">
    <Item>
      <Product>1742008805</Product> <!-- GOOD - Citrus Tea  -->
    </Item>
  </ModOp>
  <!-- END ADD GOOD TO OldLevants_ImportGoods + Docklands_ImporterGoods -->
```

## Building Citrus Tea

Now that we have our product and we made sure the product is visible in our kontor, it is time to make the building that will actually produce our new product.

### The code of the building

We are going to make a production building. This building has a certain template: **&lt;Template>FactoryBuilding7&lt;/Template>**

We can go to our assets.xml file and search for multiple examples to see how this template is structured and what kind of possibilities it has. Let's go over the most common parts.

#### Standard

This part should not have any secrets anymore for us. We can easily fill this in with all our info.

```XML
<Standard>
    <GUID>1742008809</GUID>
    <Name>Citrus Tea Dryer</Name>
    <IconFilename>data/modgraphics/icons/citrus-tea.png</IconFilename>
    <InfoDescription>1742008810</InfoDescription>
</Standard>
```

#### Building

This part contains a couple of different parts.

##### BuildModeRandomRotation

```XML
<BuildModeRandomRotation>90</BuildModeRandomRotation>
```

Define how many degrees you can rotate the building when placing the building.

- 90
- 180

###### BuildingType

```XML
<BuildingType>Residence</BuildingType>
```

The type of building can be:

- Residence
- Logistic (Warehouse)
- Warehouse (Trading Post)
- BuildingModule (Silo, Tractor)
- Factory (Hacienda alternative production factories)
- Other (Campaign and scenario specific buildings)

##### AssociatedRegions

```XML
<AssociatedRegions>Moderate</AssociatedRegions>
```

Defines in which region we can build this building.

Regions:

- Moderate
- Colony01
- Arctic
- Africa

###### BuildingCategoryName

```XML
<BuildingCategoryName>100000</BuildingCategoryName>
```

The category of the building can be:

- 100000 (Production)
- 11175 (Livestock Area)
- 100003 (Public Service)
- 2498 (Exhibition Space)
- 11169 (Electricity)
- 11149 (Ornament)
- 11150 (Harbour)
- 11151 (Infrastructure)
- 11152 (Administration) - Example: Guild House/Town Hall
- 11154 (Railway)
- 269841 (Tractor)
- 19227 (Wooden Ruins)
- 19233 (Stone Ruins)
- 19158 (Sells tickets for long voyages)
- 116953 (Abandoned Tents)
- 269960 (Agricultural Improvement)
- 134142 (Food & Drink Venue)
- 132773 (Multifactory)
- 134987 (Tourist Lodgings)
- 0 (ScenarioRuinEco)

##### TerrainType

```XML
<TerrainType>Water_Including_Coast</TerrainType>
```

Defines **special terraintypes** where the building can be placed. This can be:

- Water_Including_Coast
- Water_Excluding_Coast
- Coast
- Terrain (used in special cases)

##### SecondPartyRelevant

```XML
<SecondPartyRelevant>0</SecondPartyRelevant>
```

Disable a building for AI. This building will not be available for them.

##### Movable

```XML
<Movable>0</Movable>
```

Make it impossible to move the building/asset. Set to 0.

##### AllowChangeDirection

```XML
<AllowChangeDirection>0</AllowChangeDirection>
```

Disable changing direction. Set to 0.

##### AlternativeGrassColorAvailable

```XML
<AlternativeGrassColorAvailable>1</AlternativeGrassColorAvailable>
```

Make alternative grass available for the building. Set to 1.

#### Blocking

This is used for example for coastal buildings. To block tiles that are reachable by quay/street.

```XML
<Blocking>
    <GroundDecalAsset>100446</GroundDecalAsset> <!-- Quay street -->
    <GroundDecalInvisible>101008</GroundDecalInvisible> <!-- Quay street -->
    <GroundDecalAssetExtra>100691</GroundDecalAssetExtra> <!-- Quay street -->
    <HasBuildingBaseTiles>1</HasBuildingBaseTiles>
</Blocking>
```

#### Cost

Every building has a building cost.

```XML
<Cost>
    <Costs>
        <Item>
            <Ingredient>1010017</Ingredient> <!-- Coins -->
            <Amount>4000</Amount>
        </Item>
        <Item>
            <Ingredient>1010196</Ingredient> <!-- Timber -->
            <Amount>10</Amount>
        </Item>
        <Item>
            <Ingredient>1010205</Ingredient> <!-- Bricks -->
            <Amount>5</Amount>
        </Item>
        <Item>
            <Ingredient>1010218</Ingredient> <!-- Steal Beams -->
            <Amount>5</Amount>
        </Item>
        <Item>
            <Ingredient>1010207</Ingredient> <!-- Windows -->
        </Item>
        <Item>
            <Ingredient>1010202</Ingredient> <!-- Reinforced concrete -->
        </Item>
    </Costs>
</Cost>
```

WILL UPDATE THIS FURTHER, STAY TUNED!
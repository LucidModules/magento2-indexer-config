# Module LucidModules_IndexerConfig
This module shows how to optimize Magento 2 indexer batch size.
Use it as a template for your e-commerce and experiment with batch size to match your needs.

Larger batches will consume more server resources, but might be processed faster under specific circumstances.

## Usage
Update batchRowsCount value in the `etc/di.xml` file according to your needs. 
Here the batch size is set to 1000 for configurable products.
This means that one indexer iteration will load up to 1000 products.
```xml
<type name="Magento\Catalog\Model\ResourceModel\Product\Indexer\Price\BatchSizeCalculator">
    <arguments>
        <argument name="batchRowsCount" xsi:type="array">
            <item name="configurable" xsi:type="number">1000</item>
        </argument>
    </arguments>
</type>
```

This module contains only basic product types. Beyond `configurable` there are other product types: `grouped` and `bundle`.
Look up similar class names in Magento core di.xml files to discover other indexers batch settings.

For example, in Magento Bundle module, you can find the following config:
```xml
<type name="Magento\Catalog\Model\ResourceModel\Product\Indexer\Price\BatchSizeCalculator">
    <arguments>
        <argument name="estimators" xsi:type="array">
            <item name="bundle" xsi:type="object">Magento\Catalog\Model\Indexer\Price\CompositeProductBatchSizeManagement</item>
        </argument>
        <argument name="batchSizeAdjusters" xsi:type="array">
            <item name="bundle" xsi:type="object">Magento\Catalog\Model\ResourceModel\Product\Indexer\Price\CompositeProductBatchSizeAdjuster</item>
        </argument>
    </arguments>
</type>
```

Copy desired configuration to your module di.xml. Remember to add dependency on new module in the `etc/module.xml`:
```xml
<sequence>
    <module name="Magento_Catalog"/>
    <module name="Magento_CatalogInventory"/>
    <module name="Magento_Bundle"/>
</sequence>
```

## Installation with Composer
`composer require lucid-modules/module-indexer-config`

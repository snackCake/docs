# Dynamic Pricing Configuration

Dynamic Pricing enables you to control pricing for SKUs based on external criteria specifc to a given request instead of simply reading from the BLC\_SKU table. For example, you may have location based pricing determined by a user's currently selected store.

This pricing model is based on ThreadLocal attributes that are set up in a filter in the request and then utilized later.

The first step is to create our filter that will add certain attributes to the current request's sku pricing considerations

```java
@Component("myDynamicSkuPricingFilter")
public class MyDynamicSkuPricingFilter extends AbstractDynamicSkuPricingFilter {
    
    @Resource(name="myDynamicSkuPricingService")
    protected DynamicSkuPricingService skuPricingService;
    
    @Override
    public DynamicSkuPricingService getDynamicSkuPricingService(ServletRequest arg0) {
        return skuPricingService;
    }

    @SuppressWarnings({ "rawtypes", "unchecked" })
    @Override
    public HashMap getPricingConsiderations(ServletRequest request) {
        HashMap pricingConsiderations = new HashMap();

        // Put some attribute into this map for use. For example, the current
        // customer or the currently selected store
        
        return pricingConsiderations;
    }
}
```

Secondly, you will need to implement the `DynamicSkuPricingService` interface. Here is a sample implementation

```java
@Service("myDynamicSkuPricingService")
public class MyDynamicSkuPricingServiceImpl implements DynamicSkuPricingService {
    
    @Override
    public DynamicSkuPrices getSkuPrices(Sku sku, @SuppressWarnings("rawtypes") HashMap skuPricingConsiderations) {

        Money retailPrice;
        Money salePrice;

        // Code here to determine what the retail price should be based on the
        // current sku and the skuPricingConsiderations
        
        DynamicSkuPrices prices = new DynamicSkuPrices();
        prices.setRetailPrice(retailPrice);
        prices.setSalePrice(salePrice);
        return prices;
    }
}

```

Lastly, we need to instruct the application to use our filter, which means that we need to add it to all applicable filter chains. First, let's take a look at the `blPostSecurityFilterChain`. This filter chain is configured in `applicationContext-filter.xml` in the site project, and will typically look something like this:

```xml
<bean id="blPostSecurityFilterChain" class="org.springframework.security.web.FilterChainProxy">
    <sec:filter-chain-map request-matcher="ant">
        <sec:filter-chain pattern="/**" filters="
           blRequestFilter,
           blCustomerStateFilter,
           blTranslationFilter,
           blCartStateFilter,
           blURLHandlerFilter"/>
    </sec:filter-chain-map>
</bean>
```

We'll add our new filter just before the `blCartStateFilter` so that the appropriate pricing context is set before the cart is loaded:

```xml
<bean id="blPostSecurityFilterChain" class="org.springframework.security.web.FilterChainProxy">
    <sec:filter-chain-map request-matcher="ant">
        <sec:filter-chain pattern="/**" filters="
           blRequestFilter,
           blCustomerStateFilter,
           blTranslationFilter,
           myDynamicSkuPricingFilter,
           blCartStateFilter,
           blURLHandlerFilter"/>
    </sec:filter-chain-map>
</bean>
```

> Note: If you are using REST services, you'll also want to add this filter to the `blRestPostSecurityFilterChain` filter chain before the `blCartStateFilter`.

And that's it! Any time a call is made to .getPrice() on a Sku, it will invoke your DynamicSkuPricingService and retrieve your dynamic prices!

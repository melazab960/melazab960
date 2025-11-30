#### اضافة عميل جديد

![](C:\Users\hp\Desktop\images\0.jpg)

```python
def save(self, *args, **kwargs):
        self.slug = slugify(self.name)

        
        if self.pk:      # اذا كان المنتج موجود بالفعل 
            try:
                old_instance = Product.objects.get(pk=self.pk)    #  هات المنتج القديم   
                if old_instance.brand != self.brand or old_instance.mainitem  != self.mainitem:    #  هل البراند والبند الرئيسي الجديد لا يساوي القديم معني كده حصل تعديل في ايهم 
                   self.code = f"{self.mainitem.code}-{self.brand.code}-{old_instance.code_no}"   # سجل الكود مره تانية بالبيانات الجديدة

            except Product.DoesNotExist:
                pass
        

        elif not self.code:
            last_product = Product.objects.filter(
                mainitem=self.mainitem,
                brand=self.brand
            ).order_by('-id').first()

            if last_product and last_product.code and last_product.code_no:
                

                next_number = int(last_product.code_no) + 1 


            else:
                next_number = 1

            self.code = f"{self.mainitem.code}-{self.brand.code}-{next_number}"
            self.code_no=next_number

        
        

        super().save(*args, **kwargs)
```


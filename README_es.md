# Extensión StarUML para autogenerar modelos Django dado un diagrama UML E/R

## Características

- Generación de clases para los modelos.
- Generar los atributos, en función al tipo asignado usa la correspondiente clase ``Model Field de Djando``.
- Representar herencia, en caso de no existir, por defecto hereda de ``models.Model``.
- Generar relaciones, añadiendo ``OneToOne``, ``ForeingKey`` y ``ManyToMany``.
- Añadir atributos ``Meta`` en funcion a los ``tags`` definidos.
- Añadir parámetros adicionales a los los atributos y relaciones en función a los ``tags`` definidos.

## Requisitos

Para que funcione la generación de ``Mode Field`` en función al tipo, es necesario tener en el diagrama de modelos un esquema adicional con clases llamadas como los tipos básicos.

![](https://raw.githubusercontent.com/josemlp91/staruml-django/master/docs/images/basic_types.png)

## Ejemplo


![](https://raw.githubusercontent.com/josemlp91/staruml-django/master/docs/images/example_diagram.png)


### AbstactStudent Model
```python
#-*- coding: utf-8 -*-

from django.db import models

class AbstractStudent(models.Model):
    class Meta:
        verbose_name='foo'

    type = models.CharField()
```


### Student Model
```python
#-*- coding: utf-8 -*-

from django.db import models
from AbstractStudent import AbstractStudent

class Student(AbstractStudent):
    class Meta:
        verbose_name='Estudiante'

    name = models.CharField(max_length=1024, verbose_name='nombre', null=True)
    surname = models.CharField()
    birthdate = models.DateField()

    school = models.ForeingKey('School', on_delete=models.PROTECT)
    teachers = models.ManyToMany('Teacher')
    expedient = models.OneToOne('Expedient')

    @property
    def age(self, ):
        pass

```

### Teacher Model
```python
#-*- coding: utf-8 -*-

from django.db import models

class Teacher(models.Model):
    class Meta:
        pass

    name = models.CharField()
    speciality = models.CharField()

    school = models.ForeingKey('School', related_name='teachers', on_delete=models.PROTECT)

```

### School Model


```python
#-*- coding: utf-8 -*-

from django.db import models

class School(models.Model):
    class Meta:
        pass

    name = models.CharField()
    address = models.CharField()
```

### Expedient Model

```python
#-*- coding: utf-8 -*-
from django.db import models

class Expedient(models.Model):
    class Meta:
        pass

    qualification = None

```
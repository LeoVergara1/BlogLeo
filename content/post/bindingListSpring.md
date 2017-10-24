---
date: 2017-06-14
categories:
- projects
- releases
- code
- spring
tags:
- responsive
- Experience
- help
keywords:
- disqus
- google
- gravatar
autoThumbnailImage: false
thumbnailImagePosition: "top"
thumbnailImage: images/frameworks/Spring.png
coverImage: /images/frameworks/Spring2.png
title: "Mapear lista dinámica con spring 4"
metaAlignment: center
---

Este problema lo planteo para cuando nos encontramos en una situación donde deseamos mapear un formulario dinamico y sus datos sean ligados (binding) con un controller, de esta manera nos evitamos tener que estar capturando cada elemento de un formulario conforme va creciendo.

Lo primero es tener bien definidos los nombres de un pojo con los inpust de tu formulario como se muestra en el siguiente ejemplo.

<!--more-->
``` java
package mx.edu.ebc.api.pojo

class Commissions{
  Integer rolId
  Integer credit
  Integer cash
}
```

Posteriormente a esto crearemos un segundo pojo donde crearemos a lista de comandos.

``` java
package mx.edu.ebc.api.command
import mx.edu.ebc.api.pojo.Commissions

class ListCommissionsCommand {
  List<Commissions> commands = []

}
```

Una vez tenido esto pasaremos al mapeo de la lista en con controller, lo cual debe quedar de la siguiente manera, para que spring sepa que va recibir una lista de comandos que hemos ya declarado con anterioridad.

``` java
@RequestMapping(value = "/update", method = RequestMethod.POST )
  ModelAndView saveCommission(@ModelAttribute("commissions") ListCommissionsCommand commissionsList) {
  commissionsList.commands.each{ commission ->
  commissionsService.update(commission)
  }
  return new ModelAndView("redirect:");
}
```

Es necesario que cuando declaremos el modelo de atributo este sea el mismo que utilizemos en el formulario para que así pueda ser ligado(Binding) de mandera exitosa, de tal forma que dicho formulario quede de la siguiente manera.

``` html
  <form th:object="${commissionsList}" th:action="@{/commissions/update}" method="post" id="formCommissions">
  <div class="col-lg-12">
  <table class="table table-bordered">
  <tbody>
  <tr style="background-color:#0b6c99; color:white;">
  <th>Rol</th>
  <th colspan="2" class="text-center">Porcentaje de comisión</th>
  </tr>
  <tr>
  <td></td>
  <td style="background-color:#f9f9f9;" class="text-center"><b>Crédito</b></td>
  <td style="background-color:#f2f2f2;" class="text-center"><b>Contado</b></td>
  </tr>
  <tr th:each= "commission, element: ${commissionsList.commands}" >
  <td><input th:name="|commands[${element.index}].rolId|" class="form-control" id="exampleInputAmount" placeholder="" th:value="${commission.rolId}" type="hidden" /><b th:text="${mapRol[commission.rolId]}" ></b></td>
  <td style="background-color:#f9f9f9;"><input th:name="|commands[${element.index}].credit|" type="number" max="99" maxlength="2" class="form-control" placeholder="" th:value="${commission.credit}" pattern="[0-9]{1,2}" /></td>
  <td style="background-color:#f2f2f2;"><input th:name="|commands[${element.index}].cash|" type="number" max="99" maxlength="2" class="form-control" placeholder="" th:value="${commission.cash}"/></td>
  </tr>
  </tbody>
  </table>
  </div>
  <br class="clear"/>
  <hr/>
  <div class="col-md-9 col-lg-9"></div>
  <div class="col-md-3 col-lg-3">
  <button class="btn btn-success btn-block btn-sm" id="buttonSave"><i class="fa fa-check" aria-hidden="true" ></i> Guardar</button>
  </div>
  </form>
```


Como se puede apreciar indicamos nuestro objeto al cual hace referencia y en el each vamos hacer referencia a la lista que tenemos declarada en este pojo en cual es commissionsList.commands,  es importante destacar que el nombre de nuestros inputs debe ser exactamente igual a cada elemento de la lista ejemplo ->

``` html
<td><input th:name="|commands[${element.index}].rolId|" class="form-control" id="exampleInputAmount" placeholder="" th:value="${commission.rolId}" type="hidden" /><b th:text="${mapRol[commission.rolId]}" ></b></td>

En el navegador se vería de la siguiente manera:

<td><input name="commands[0].rolId" class="form-control" id="exampleInputAmount" placeholder="" value="764" type="hidden" /><b  text="Un mapa que no es importante" ></b></td>
```

Lo importante es que cada elemento vaya aumentando según el index para que obtengamos un comando por cada ciclo del objeto.
<!--more-->

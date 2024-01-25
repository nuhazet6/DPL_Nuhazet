# ÍNDICE

+ [Material empleado](#id1)
+ [Desarrollo](#id2)

# ***Material empleado***. <a name="id1"></a>

- Nginx
- PostgreSQL + datos
- pgAdmin
- CertBot

## ***Desarrollo***. <a name="id2"></a>
### Instalación
``` curl -O --output-dir /tmp \
https://download.java.net/java/GA/jdk19.0.1/afdd2e245b014143b62ccb916125e3ce/10/GPL/openjdk-19.0.1_linux-x64_bin.tar.gz ```  
``` sudo tar -xzvf /tmp/openjdk-19.0.1_linux-x64_bin.tar.gz \
--one-top-level=/usr/lib/jvm ```  
extablecemos la variables de entorno:  
``` sudo nano /etc/profile.d/jdk_home.sh ```  
Contenido:  
``` #!/bin/sh
export JAVA_HOME=/usr/lib/jvm/jdk-19.0.1/
export PATH=$JAVA_HOME/bin:$PATH ```  
``` sudo update-alternatives --install \
"/usr/bin/java" "java" "/usr/lib/jvm/jdk-19.0.1/bin/java" 0 ```  
``` sudo update-alternatives --install \
"/usr/bin/javac" "javac" "/usr/lib/jvm/jdk-19.0.1/bin/javac" 0 ```  
``` sudo update-alternatives --set java \
/usr/lib/jvm/jdk-19.0.1/bin/java ```  
``` sudo update-alternatives --set javac \
/usr/lib/jvm/jdk-19.0.1/bin/javac ```  
``` sudo apt install -y zip ```  
``` curl -s https://get.sdkman.io | bash ```  
``` source "$HOME/.sdkman/bin/sdkman-init.sh" ```  
``` sdk install springboot ```  
``` sdk install maven ```
### Aplicación
``` spring init \
--build=maven \
--dependencies=web \
--group=edu.dpl \
--name=travelroad \
--description=TravelRoad \
travelroad_spring ```
dentro de travel_spring:
``` mkdir -p src/main/java/edu/dpl/travelroad/controllers ```  
``` touch src/main/java/edu/dpl/travelroad/controllers/HomeController.java ```  
``` mkdir -p src/main/java/edu/dpl/travelroad/models ```  
```  touch src/main/java/edu/dpl/travelroad/models/Place.java ```  
``` mkdir -p src/main/java/edu/dpl/travelroad/repositories ```  
``` touch src/main/java/edu/dpl/travelroad/repositories/PlaceRepository.java ```  
``` touch src/main/resources/templates/home.html ```
Modificamos el controlador:
``` nano src/main/java/edu/dpl/travelroad/controllers/HomeController.java ```  
```
package edu.dpl.travelroad.controllers;

import edu.dpl.travelroad.models.Place;
import edu.dpl.travelroad.repositories.PlaceRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {
    private final PlaceRepository placeRepository;

    @Autowired
    public HomeController(PlaceRepository placeRepository) {
        this.placeRepository = placeRepository;
    }

    @GetMapping("/")
    public String home(Model model) {
        model.addAttribute("wished", placeRepository.findByVisited(false));
        model.addAttribute("visited", placeRepository.findByVisited(true));
        return "home";  // home.html
    }
    @GetMapping("/wished")
    public String wished(Model model) {
        model.addAttribute("wished", placeRepository.findByVisited(false));
        return "wished";  // wished.html
    }
    @GetMapping("/visited")
    public String visited(Model model) {
        model.addAttribute("visited", placeRepository.findByVisited(true));
        return "visited";  // visited.html
    }
}
 ```  
```  ```  
```  ``` 



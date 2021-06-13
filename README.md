# OmegaT theme plugin skeleton

This is OmegaT theme plugin skeleton to provide additional theme settings for OmegaT 5.6.0 and later.

## How to get skeleton into your project

It is recommend to use `Use this template` button on upper-right side of github project page,
to create your project repository.

![](https://docs.github.com/assets/images/help/repository/use-this-template-button.png)

## Gradle DSL

There is a Gradle build script in skeleton with Kotlin DSL(`build.gradle.kts`)

## Where you should change?

Here is a hint for modifications.

- Source code: `src/main/java/*`
- Project.name in `settings.gradle`
- Properties: description, title, website and category
- Plugin Main class name in `build.gradle.kts`.
- Coding rules: `config/checkstyle/checkstyle.xml`

## Build system

This skeleton use a Gradle build system as same as OmegaT version 4.3.0 and later.

## Dependency

OmegaT and dependencies are located on remote mavenCentral repositories.
It is necessary to connect the internet to compile your project.

Current skeleton refers OmegaT 5.6.0.

All complex configurations to refer OmegaT core are handled by
`gradle-omegat-plugin`.

## FatJar(ShadowJar)

OmegaT considered a plugin is a single jar file. If it is depend on some libraries, 
you should ship your plugin with these libraries.
It is why generating a FatJar, a single jar file with all runtime dependencies
which is not provided with OmegaT.

`gradle-omegat-plugin` offers special gradle configuration `ParckIntoJar`.
When specified it, gradle will generate a proper FatJar for you.


## Where is a built artifact?

You can find distribution files in `build/distributions/*.zip`.
Also you can find jar files at `build/libs/`


## Developer note

### OmegaT defaults

Theme plugins can get OmegaT defaults through `DefaultFlatTheme#setDefaults` or
`DefaultClassicTheme#setDefaults`, then customize and override it by following way.

```java
public UIDefaults getDefaults(){
        UIDefaults defaults=super.getDefaults();
        // get OmegaT defaults
        defaults=DefaultFlatTheme.setDefaults(defaults);
        defaults.put("OmegaTBorder.color",borderColor);
        return defaults;
}
```

### Default values of theme

Swing `UIManager` manages three "layers" of "defaults" (= sets of key-value settings). From the Javadoc:

1. Developer defaults. With few exceptions Swing does not alter the developer defaults; these are intended to be modified and used by the developer.
2. Look and feel defaults. The look and feel defaults are supplied by the look and feel at the time it is installed as the current look and feel (setLookAndFeel() is invoked). The look and feel defaults can be obtained using the getLookAndFeelDefaults() method.
3. Sytem defaults. The system defaults are provided by Swing.
   
In `setDefaults` implementation we can only set values on layer 2 (LAF defaults).
However VLDocking's installUI puts all of its settings into layer 1.
We can't override the VLDocking defaults from layer 2, so we have no choice
but to put them in layer 1 via UIManager#put*.

The correct solution is to change VLDocking: it should let you install
its defaults into layer 2. But in the meantime we can work around it
by using UIManager#put* in our setDefaults method.

### When customize `Dockview`

You should use `UIManager#put` in the meantime like as follows;

```
UIManager.put("DockViewTitleBar.border", new MatteBorder(1, 1, 1, 1, borderColor));
```

## License

The plugin skeleton is distributed under GNU General Public License 3 or later.


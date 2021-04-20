Adding a GUI
We’re going to add a simple parameter to our `SpherePawn` class named `Health`. This will automatically tick down every frame, and we’ll create a HUD with an animated bar to show the current value. We’ll then also add a constructor method to our `GameMode` subclass that will place our GUI on the screen. Last but not least, we’ll make the GUI itself!

### Adding the C++ for our SpherePawn
This one is pretty simple. We’re going to add a property named Health, set its initial value in the `SpherePawn` constructor, and then decrease it slightly on the `Tick` method of `SpherePawn`.

In our `SpherePawn.h` header, add the following public property:

```c++
UPROPERTY(BlueprintReadOnly)
float Health;
``` 

Blueprints will be able to read this value, but only our C++ methods will be able to change it.

In our `SpherePawn` C++ file, set the initial value of `Health` to be `1.f` in the constructor. Change the `Tick` method, to the following:

```c++
void ASpherePawn::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);
	Health -= .0001f;
}
```

OK, that’s all we need to do here! The SpherePawn now has a Health property that will automatically decrease on each frame, and this property is set to be readable by Blueprints. Now, on to use the Unreal Motion Graphics framework.

### Modifying our build file
Go ahead and open the C# build file for your project, located alongside the rest of your project’s source code. This configures many of the default libraries that are included in your build. Add “UMG” to the first dependency module configuration, so it reads:

```c++
PublicDependencyModuleNames.AddRange(new string[] { “Core”, “CoreUObject”, “Engine”, “InputCore”, “UMG” });
```

Next, uncomment the Slate UI line immediately beneath the one you just edited. Nothing else to see here; on to editing our game mode.

### Editing our GameMode subclass
The GameMode is responsible for broadly managing the game, including displaying relevant statistics via HUD, managing win/lose conditions and more. This is the class we’ll use to go ahead and display the GUI we create using the Unreal Motion Graphics editor (UMG).

Go ahead and open up your project’s `GameModeBase` subclass header;  the Unreal project template will have made this for you automatically. Add the following public properties to your class:

```c++
public:
	UPROPERTY(EditDefaultsOnly, BlueprintReadWrite, Category=“Health”)
	TSubclassOf<class UUserWidget> HUDWidgetClass;
	
	UPROPERTY()
	class UUserWidget * CurrentWidget;

	virtual void BeginPlay() override;
```

Next, add override the `BeginPlay` method in the C++ file and include the UserWidget header file. Make sure to change your project name where appropriate, and don’t remove the A prefix in the class name that denotes this an Actor:	

```c++
#include “YOURPROJECTNAMEGameModeBase.h”
#include “Blueprint/UserWidget.h”
```

```c++
void AYOURPROJECTNAMEHEREGameModeBase::BeginPlay() {
    Super::BeginPlay();

    CurrentWidget = CreateWidget<UUserWidget>(GetWorld(), HUDWidgetClass);
    CurrentWidget->AddToViewport();
}
```

### Create a Blueprint to display our Health.
In the editor content browser, 	right-click and choose User Interface > Widget Blueprint. Name it “HealthHUD”.

In the designer view, layout a simple Progress Bar widget and place an accompanying text label. With the progress bar you just added selected, choose Details > Progress > Bind. This will create a new binding for your element that will enable you to update your progress bar in a blueprint.

In the resulting blueprint graph view, create a [blueprint like this image](https://github.com/imgd-4000-2020/syllabus-and-notes/blob/master/images/day4/widget_blueprint.png)

Compile and save this blueprint.

### Link everything together

1. Create a blueprint subclass of our `GameModeBase`. Name it “GameMode_BP”.
2. Double click on it and set the Health > HUDWidgetClass to be our “Health HUD”.
3. Choose Edit > Project Settings > Maps and Modes > Default Modes > Default GameMode and set it to be “GameMode_BP” using the associated dropdown menu.

Try Compiling the C++ code if you haven’t done so in a while, and then run Play. You should see a health bar counting down!

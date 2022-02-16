# ue4-code-convention

<a id="toc"></a>
<a id="8"></a>
<a id="codeconv"></a>
## 0. Code Convention ![#](https://img.shields.io/badge/lint-unsupported-red.svg)

This document will focus on Code convention not covered in the official UE4 C++ guideline. (https://docs.unrealengine.com/en-US/Programming/Development/CodingStandard/index.html)

### Sections

> 0.1 [Future Features in code marking](#futurefeature)

> 0.2 [Variable passing in function declarations](#varpassing)

> 0.3 [Code documentation](#documentation)

<a id="0.1"></a>
<a id="futurefeature"></a>
### 0.1 Future Features in code marking ![#](https://img.shields.io/badge/lint-unsupported-red.svg)

When you are creating a temporary solution for code that is not ported to C++ yet or if you know something will have to change later on you add a comment tag on it like this:

```
// TODO: replace later with this blibloe (https://cyborn.atlassian.net/browse/HUB-212)
```

This way we can easily identify at two points if we still got refactoring work to go in both the VS project as in our project management dashboard.

<a id="0.2"></a>
<a id="varpassing"></a>
### 0.2 Variable passing in function declarations ![#](https://img.shields.io/badge/lint-unsupported-red.svg)

Function declarations that take variables in are to define these variables with cammel case naming to avoid variable hiding.
For boolean parameters, ```bool bIsABool``` becomes ```bool isABool```.

__This rule also applies for inherited functions.__

Good:
```cpp
void HubrisClass::HubrisFunction(int thisIsVarA, int thisIsVarB, bool isThisAFunction)
{
	...
}
```

Bad:
```cpp
void HubrisClass::HubrisFunction(int ThisIsVarA, int ThisIsVarB, bool bIsThisAFunction)
{
	...
}
```

<a id="0.3"></a>
<a id="documentation"></a>
### 0.3 Code Documentation ![#](https://img.shields.io/badge/lint-unsupported-red.svg)

<a id="0.3.1"></a>
<a id="documentation-generation"></a>
#### 0.3.1 Comment style for Documentation Generation Tool

Cyborn utilizes a modified version of the <a href="https://github.com/kamrann/KantanDocGenPlugin">Kantan Doc gen tool</a> For automatically creating documentation. We currently keep track of following things:

Native:
* UClasses
* UFunctions if BlueprintCallable
* UMembers (Private | Read | Read/Write)

Blueprinted:
* Blueprint Functions
* Blueprint Variables

<a id="0.3.1.1"></a>
<a id="documentation-generation-class"></a>
#### 0.3.1.1 Classes

You can add tags to a class declaration, noting the state of a certain attribute of the class.
Currently these tags are supported:

**@SaveGame** _Ready | ToDo | None_<br/>

Stating if this class should be saveable or not, and if it is or is not implemented yet.

**@Refactor** _Ready | ToDo_<br/>

Stating if this class is already refactored or still needs a pass.

```cpp
/**
 * The main pawn for VR gameplay.
 * Contains all input handling and main gameplay components for movement and scene interaction.
 * This pawn should never directly reference any game objects that aren't persistent.
 * @SaveGame Ready
 * @Refactor Ready
**/
UCLASS(meta = (ShortTooltip = "The main pawn for VR gameplay. Contains all components necessary for world interaction."))
class HUBRISVR_API ABaseVRPawn : public ABasePawn, public IEMSActorSaveInterface
```

The  `meta = (ShortTooltip = "")` to the **UCLASS** macro adds a short description that can be read in blueprints and in the index list of the documentation.

<a id="0.3.1.2"></a>
<a id="documentation-generation-function"></a>
#### 0.3.1.2 Functions

Functions tagged with **BlueprintCallable** should include the following function declaration.

```cpp
/**
 * Function description goes here.
 * You can use multiple lines.
 *
 * @param ParamX Description of parameter ParamX.
 * @param ParamY Description of parameter ParamY.
 * @return Description of function return value.
 */
UFUNCTION(BlueprintCallable, ...)
int32 SomeFunction(FString ParamX, bool ParamY);
```

Blueprints should properly fill in the description for all the In/Outputs and function description boxes.

<a id="0.3.1.3"></a>
<a id="documentation-generation-variable"></a>
#### 0.3.1.3 Member Variables

Accessible class members should describe in a comment what they are used for. The tool will scrap the single line comment above the member declaration.

```cpp
// Keeps track of the ammount of cycles we were asleep
UPROPERTY(EditAnywhere,BLueprintReadWrite,Category="Physics state")
uint32 SleepCycleCounter;
```

<a id="0.3.2"></a>
<a id="documentation-style"></a>
#### 0.3.2 Comment style

In **class, function or variable declarations**, multiline comments blocks should always follow the Javadoc comment style.<br/>
Single line comments can use the Javadoc as well as C++ style
```cpp
/**
 * This is a single line comment.
 */
void ExampleFunction();

// This single line comment is also allowed.
void ExampleFunction();

/**
 * This is a javadoc comment style block.
 * It can contain as many lines as needed.
 * See, three lines already!
 */
void ExampleFunction();
```
In **function definitions**, both single and multiline comments blocks should be C++ style comments.<br/>
These comments should always give extra information that is not contained within the code itself.

Good:
```cpp
TArray<int> MyClass::ExampleFunction(const TArray<int>& IntList)
{
	TArray<int> ValidInts;
	
	// Filter out invalid numbers in a prepass.
	// This avoids duplicating this filter operation when processing the list multithreaded.
	for (int Number : IntList)
	{
		if(MatchesRequirements(Number))
		{
			ValidInts.Add(Number);
		}
	}

	return ValidInts;
}
```

Bad: 
```cpp
TArray<int> MyClass::ExampleFunction(const TArray<int>& IntList)
{
	TArray<int> ValidInts;
	
	// We loop through all ints and store the relevant ones.
	for (int Number : IntList)
	{
		if(MatchesRequirements(Number))
		{
			ValidInts.Add(Number);
		}
	}

	// Return the new filtered array.
	return ValidInts;
}
```
<a id="0.3.3"></a>
<a id="documentation-members"></a>
#### 0.3.3 Class Members

Accessible class members should always be properly documented.
- Document every public class member (= both functions and variables).
- Document every blueprint-accessible protected or private class member.


**[⬆ Back to Top](#toc)**
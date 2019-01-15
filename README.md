# Unity-Guidelines

The purpose of this document is to describe a way to organize a project on Unity in order to keep it
clean and organized during development.

The organization method presented here is based on three principles: alphabetical sorting,
dependency grouping, namespace.

The ideas behind these principles is to always be able to find something in a logical way and to
be able to work on something without searching everywhere in your project.

Quick note: Parent folders or parent elements are always named without 's' because this is
a namespace and not a collection.

## Architecture

### Asset Naming (Optional)

Each **asset you create inside Unity** must be prefix with there type symbol and an '**_**'.

Example:

    Animation Clip  -> AnC
    Animator        -> A_
    Audio Clip      -> AuC
    Material        -> M_
    Prefab          -> P_
    Scene           -> S_
    Terrain         -> T_
    Terrain Layer   -> TL_
    ...

In case of two types share the same symbol, add their second letter.

    Animation Clip  -> AnC // n has been added
    Audio Clip      -> AuC // u has been added

### Directory Structure

*All the "Unity" folders are optionals*

    Animation
        Animator
        Clip
    Audio
        Ambient
        Mixer
        Music
        SFX
    Character
    Editor Default Resources        // Unity
    Environment
        Terrain
            Desert
                Layer
                Texture
        Vegetation
        Vehicule
        ...
    Gizmos                          // Unity
    Package                         // All the 3rd-Party asstes
        Animation
        Audio
            Ambient
            Music
            SFX
        Character
        Environment
            Vegetation
            Vehicule
            ...
        Font
        Template
        Tool
            AI
            Terrain
            ...
        UI
            GUI
            HUD
        VFX
            Particule
            Shader
    Physic Material
    Plugins                         // Unity
    Resources                       // Unity
    Scene
        NameOfYourScene
            Light                   // Generated light information
            T_YourSceneName.asset   // The terrain related to your scene
            YourScene.unity
    Script
        Component
            Base                    // Abstract component
            Classic                 // Component aiming to be used on visible GameObject
            Daemon                  // Component aiming to run a background task on a non visible GameObject
        Debug
        Framework
        Helper                      // Helper Class
        Library
        Template
        Utility                     // Utility Class (static)
    Standard Assets                 // Unity
    StreamingAssets                 // Unity
    VFX
        Particule
        Shader

### Game Hierarchy

For each parent there transform must be **Reset**.

    @Daemon             // Non visible GameObject that run a background task (e.g. GameManager)
    _Temporary          // Generated GameObject during play mode
    Camera
    Character
        Ally
        Enemy
        NPC
        Player
    Environment         // Visible GameObject that's not a character
        Dynamic         // Movable
        Static          // Bake
    Light
        Area
        Directional
        Point
        Spot
    Trigger Area
    UI
        GUI             // Interactive UI
        HUD             // Information UI

## Coding Style

### Scope

##### Brackets

Every scope must be wrapped by brackets, even if it's one line of code

Example
    
    if ( condition )
    {
        oneLineOfCode;
    }

#### Condition

Every condition must be wrapped by space and if you have more than one condition, you need to had parenthesis to your
conditions

Example #1

    if ( condition )
    {
        // Your code
    }

Example #2

    while ( (condition1) && (condition2) )
    {
        // Your code
    }

#### Variable

In each scope, it's recommended to divided your variables declaration and your methods.

Example

    ..... MyMagicMethod()
    {
        int columnIndex = 0;
        int rowIndex = 0;
        
        if ( columnIndex == 0 )
        {
            int deltaIndex = 42;
            
            YourStuff...        
        } 
    }

### Class

#### Attribute

- public: Start with an uppercase *(e.g. Position)*
- private: Start with an '_' than a lowercase *(e.g. _rotation)*
- protected: Start with an '_' than an uppercase *(e.g. _Scale)*

#### Inheritance

- Abstract: If your class aim to be inherited without being instantiated
- Sealed: If your class does not aim to be inherited

### Method

#### Name

Start with an action verb (e.g. UpdateXXX) and an uppercase

#### Parameter

Start with a lowercase.

    out parameter: Start with 'out' (e.g. outRaycastHit)

#### Variable

Start with a lowercase.

    bool: Start with a 'b' as prefix (e.g. bIsOpen)

#### Naming

- Abstract: Always prefix with a 'A' *(e.g. ACharacter)*
- Interface: Should always be prefix with 'I' *(e.g. IDamageable)*

If your class aim to be the cogs of an object, add 'System' as suffix *(e.g. InventorySystem)*.
By convention, a System represent the cogs and a Mechanism represent a collection of System.

### Property

#### Naming

    Getter: Name as GetYourPropertyName
    Setter: Name as SetYourPropertyName (But it's weird)
    Getter/Setter: Name as YourPropertyName

#### Scope

    Public: If you don't want the reference to be change
    Private: To perform a specific action when getting or setting your attribute
    Protected: same effect as Private and impact children

#### Structure

In order to keep the code organize, your class should look like that with the comments:

    ///////////////////////////////
    ////////// Attribute //////////
    ///////////////////////////////

    public Vector3 Position;
    private Vector3 _rotation;
    protected Vector3 _Scale;

    //////////////////////////////
    ////////// Property //////////
    //////////////////////////////

    public Vector3 GetRotation => _rotation;

    ////////////////////////////
    ////////// Method //////////
    ////////////////////////////

    /////////////////////////
    ////////// API //////////

    ////////// Position Action //////////

    public void InvertPosition()
    {
    }

    ////////// Rotation Action //////////

    public void InvertRotation()
    {
    }

    ////////////////////////////////////////////
    ////////// MonoBehaviour Callback //////////

    private void Update()
    {
        UpdatePosition();
    }

    /////////////////////////////
    ////////// Service //////////
    
    private void UpdatePosition()
    {
    }

## Gitignore

A generic gitignore for Unity is provided in the root folder *(.gitignore)*.
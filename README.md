# Quark
Event-driven C# engine for managing game entities, positions, health, and interactions.  
The project development commenced on Sunday, October 26, 2025.

Welcome to my new attempt at "I'm creating problems for myself because I already have too much work to do". So, here you see the result of 2 days (when I first committed to the repo) of research and personal realignment on the fundamental notions of design and software architecture because I happened to drift a little from my best practices lately. As a result, I re-taught myself about ten hours of personal training on the different architectures and solid principles and I think I trained properly in order to be able to continue on a good basis. The project that I present to you today is simply a kind of game engine that I built around a new type of architecture that I discovered called service driven architecture and which, as its name suggests, consists of orchestrating its systems into different services. I've actually completely fallen in love with this concept and I think it's going to replace my ECS obsession for a few weeks while the traumatic ECS C++ flashbacks fade from my mind. As for this game engine, since this is the 4th time I've tried to talk about it, I'll try to be straight forward. So basically, I started this project this morning on a whim where I had the idea of ​​applying the different notions learned this weekend and I liked the concept of game engine so it was natural that I go there. In fact, it was a few times that I tried to do something similar but each time, I always arrived at the same point where my architecture seemed too rigid and complex for me to scale up and make a real engine of my code. Finally, this new approach pushed me to create something much more SOLID and modular, which allowed me for the first time in my career to experiment with Event and trigger systems, which allowed me to create something much bigger and which makes me much more proud, a Collision System. Okay, I know it might not be the most impressive thing, but you can see that this is my first working collision system ever. In fact, I've never managed to get to this stage, and now that I've done it, I'm looking at new goals to push my skills even further. The architecture you see here is pretty much all done by me only, with the help of documentation of course but it's still 100% out of my head and my brain, and I'm quite proud of it. I made it in 4 hours non-stop almost of programming and I finished this evening, thus pushing my project on github for personal documentation purposes and to showcase my new seasonal and probably very fleeting obsessions. If you've read my message so far, I'd like to ask you first of all what you're actually doing reading 800-word messages from a random guy on GitHub and also, I sincerely thank you for your attention, I hope you enjoyed this little bit of my daily life, I invite you to stay tuned in case I re-re-release another ECS in the coming weeks. On that note, sincerely have a very nice end of the day.

Some of the information above is slightly incorrect because I ultimately reworked the project, so no, I didn't do all of that in just 4 hours :)

```csharp
// Copyright (c) October 2025 Félix-Olivier Dumas. All rights reserved.
// Licensed under the terms described in the LICENSE file

using ConsoleTableExt;
using ImGuiGeneral;
using ImGuiNET;
using SDL2;
using System;
using System.ComponentModel;
using System.Diagnostics;
using System.Drawing;
using System.Runtime.InteropServices;
using System.Xml.Linq;
using Windows.Gaming.Input;
using Windows.Perception.Spatial;
using static SDL2.SDL;
using static System.Net.Mime.MediaTypeNames;

public static class InputKeyAdapterSDL {
    public static InputKey FromSDLKey(SDL_Keycode key) => key switch {
        SDL_Keycode.SDLK_a => InputKey.Key_A,
        SDL_Keycode.SDLK_b => InputKey.Key_B,
        SDL_Keycode.SDLK_c => InputKey.Key_C,
        SDL_Keycode.SDLK_d => InputKey.Key_D,
        SDL_Keycode.SDLK_e => InputKey.Key_E,
        SDL_Keycode.SDLK_f => InputKey.Key_F,
        SDL_Keycode.SDLK_g => InputKey.Key_G,
        SDL_Keycode.SDLK_h => InputKey.Key_H,
        SDL_Keycode.SDLK_i => InputKey.Key_I,
        SDL_Keycode.SDLK_j => InputKey.Key_J,
        SDL_Keycode.SDLK_k => InputKey.Key_K,
        SDL_Keycode.SDLK_l => InputKey.Key_L,
        SDL_Keycode.SDLK_m => InputKey.Key_M,
        SDL_Keycode.SDLK_n => InputKey.Key_N,
        SDL_Keycode.SDLK_o => InputKey.Key_O,
        SDL_Keycode.SDLK_p => InputKey.Key_P,
        SDL_Keycode.SDLK_q => InputKey.Key_Q,
        SDL_Keycode.SDLK_r => InputKey.Key_R,
        SDL_Keycode.SDLK_s => InputKey.Key_S,
        SDL_Keycode.SDLK_t => InputKey.Key_T,
        SDL_Keycode.SDLK_u => InputKey.Key_U,
        SDL_Keycode.SDLK_v => InputKey.Key_V,
        SDL_Keycode.SDLK_w => InputKey.Key_W,
        SDL_Keycode.SDLK_x => InputKey.Key_X,
        SDL_Keycode.SDLK_y => InputKey.Key_Y,
        SDL_Keycode.SDLK_z => InputKey.Key_Z,

        SDL_Keycode.SDLK_0 => InputKey.Key_Num0,
        SDL_Keycode.SDLK_1 => InputKey.Key_Num1,
        SDL_Keycode.SDLK_2 => InputKey.Key_Num2,
        SDL_Keycode.SDLK_3 => InputKey.Key_Num3,
        SDL_Keycode.SDLK_4 => InputKey.Key_Num4,
        SDL_Keycode.SDLK_5 => InputKey.Key_Num5,
        SDL_Keycode.SDLK_6 => InputKey.Key_Num6,
        SDL_Keycode.SDLK_7 => InputKey.Key_Num7,
        SDL_Keycode.SDLK_8 => InputKey.Key_Num8,
        SDL_Keycode.SDLK_9 => InputKey.Key_Num9,

        SDL_Keycode.SDLK_UP => InputKey.Key_UpArrow,
        SDL_Keycode.SDLK_DOWN => InputKey.Key_DownArrow,
        SDL_Keycode.SDLK_LEFT => InputKey.Key_LeftArrow,
        SDL_Keycode.SDLK_RIGHT => InputKey.Key_RightArrow,

        SDL_Keycode.SDLK_LSHIFT => InputKey.Key_LeftShift,
        SDL_Keycode.SDLK_RSHIFT => InputKey.Key_RightShift,
        SDL_Keycode.SDLK_LCTRL => InputKey.Key_LeftCtrl,
        SDL_Keycode.SDLK_RCTRL => InputKey.Key_RightCtrl,
        SDL_Keycode.SDLK_LALT => InputKey.Key_LeftAlt,
        SDL_Keycode.SDLK_RALT => InputKey.Key_RightAlt,
        SDL_Keycode.SDLK_CAPSLOCK => InputKey.Key_CapsLock,
        SDL_Keycode.SDLK_NUMLOCKCLEAR => InputKey.Key_NumLock,
        SDL_Keycode.SDLK_SCROLLLOCK => InputKey.Key_ScrollLock,
        SDL_Keycode.SDLK_LGUI => InputKey.Key_LeftWindows,
        SDL_Keycode.SDLK_RGUI => InputKey.Key_RightWindows,
        SDL_Keycode.SDLK_MENU => InputKey.Key_Menu,

        SDL_Keycode.SDLK_BACKSPACE => InputKey.Key_Backspace,
        SDL_Keycode.SDLK_RETURN => InputKey.Key_Enter,
        SDL_Keycode.SDLK_TAB => InputKey.Key_Tab,
        SDL_Keycode.SDLK_ESCAPE => InputKey.Key_Escape,
        SDL_Keycode.SDLK_SPACE => InputKey.Key_Space,
        SDL_Keycode.SDLK_INSERT => InputKey.Key_Insert,
        SDL_Keycode.SDLK_DELETE => InputKey.Key_Delete,
        SDL_Keycode.SDLK_HOME => InputKey.Key_Home,
        SDL_Keycode.SDLK_END => InputKey.Key_End,
        SDL_Keycode.SDLK_PAGEUP => InputKey.Key_PageUp,
        SDL_Keycode.SDLK_PAGEDOWN => InputKey.Key_PageDown,
        SDL_Keycode.SDLK_MINUS => InputKey.Key_Minus,
        SDL_Keycode.SDLK_EQUALS => InputKey.Key_Equals,
        SDL_Keycode.SDLK_LEFTBRACKET => InputKey.Key_LeftBracket,
        SDL_Keycode.SDLK_RIGHTBRACKET => InputKey.Key_RightBracket,
        SDL_Keycode.SDLK_BACKSLASH => InputKey.Key_Backslash,
        SDL_Keycode.SDLK_SEMICOLON => InputKey.Key_Semicolon,
        SDL_Keycode.SDLK_QUOTE => InputKey.Key_Quote,
        SDL_Keycode.SDLK_COMMA => InputKey.Key_Comma,
        SDL_Keycode.SDLK_PERIOD => InputKey.Key_Period,
        SDL_Keycode.SDLK_SLASH => InputKey.Key_Slash,
        SDL_Keycode.SDLK_BACKQUOTE => InputKey.Key_Tilde,

        SDL_Keycode.SDLK_KP_0 => InputKey.Key_NumPad0,
        SDL_Keycode.SDLK_KP_1 => InputKey.Key_NumPad1,
        SDL_Keycode.SDLK_KP_2 => InputKey.Key_NumPad2,
        SDL_Keycode.SDLK_KP_3 => InputKey.Key_NumPad3,
        SDL_Keycode.SDLK_KP_4 => InputKey.Key_NumPad4,
        SDL_Keycode.SDLK_KP_5 => InputKey.Key_NumPad5,
        SDL_Keycode.SDLK_KP_6 => InputKey.Key_NumPad6,
        SDL_Keycode.SDLK_KP_7 => InputKey.Key_NumPad7,
        SDL_Keycode.SDLK_KP_8 => InputKey.Key_NumPad8,
        SDL_Keycode.SDLK_KP_9 => InputKey.Key_NumPad9,
        SDL_Keycode.SDLK_KP_DIVIDE => InputKey.Key_NumPadDivide,
        SDL_Keycode.SDLK_KP_MULTIPLY => InputKey.Key_NumPadMultiply,
        SDL_Keycode.SDLK_KP_MINUS => InputKey.Key_NumPadSubtract,
        SDL_Keycode.SDLK_KP_PLUS => InputKey.Key_NumPadAdd,
        SDL_Keycode.SDLK_KP_PERIOD => InputKey.Key_NumPadDecimal,
        SDL_Keycode.SDLK_KP_ENTER => InputKey.Key_NumPadEnter,

        SDL_Keycode.SDLK_F1 => InputKey.Key_F1,
        SDL_Keycode.SDLK_F2 => InputKey.Key_F2,
        SDL_Keycode.SDLK_F3 => InputKey.Key_F3,
        SDL_Keycode.SDLK_F4 => InputKey.Key_F4,
        SDL_Keycode.SDLK_F5 => InputKey.Key_F5,
        SDL_Keycode.SDLK_F6 => InputKey.Key_F6,
        SDL_Keycode.SDLK_F7 => InputKey.Key_F7,
        SDL_Keycode.SDLK_F8 => InputKey.Key_F8,
        SDL_Keycode.SDLK_F9 => InputKey.Key_F9,
        SDL_Keycode.SDLK_F10 => InputKey.Key_F10,
        SDL_Keycode.SDLK_F11 => InputKey.Key_F11,
        SDL_Keycode.SDLK_F12 => InputKey.Key_F12,

        SDL_Keycode.SDLK_PRINTSCREEN => InputKey.Key_PrintScreen,
        SDL_Keycode.SDLK_PAUSE => InputKey.Key_Pause,
        SDL_Keycode.SDLK_CLEAR => InputKey.Key_Clear,

        _ => throw new ArgumentOutOfRangeException(nameof(key), $"SDL_Keycode not mapped: {key}")
    };

    public static InputKey FromControllerButton(SDL_GameControllerButton btnEvent) {
        SDL_GameControllerButton button = btnEvent;

        return button switch {
            SDL_GameControllerButton.SDL_CONTROLLER_BUTTON_A => InputKey.Xbox_A,
            SDL_GameControllerButton.SDL_CONTROLLER_BUTTON_B => InputKey.Xbox_B,
            SDL_GameControllerButton.SDL_CONTROLLER_BUTTON_X => InputKey.Xbox_X,
            SDL_GameControllerButton.SDL_CONTROLLER_BUTTON_Y => InputKey.Xbox_Y,
            SDL_GameControllerButton.SDL_CONTROLLER_BUTTON_BACK => InputKey.Xbox_Back,
            SDL_GameControllerButton.SDL_CONTROLLER_BUTTON_START => InputKey.Xbox_Start,
            SDL_GameControllerButton.SDL_CONTROLLER_BUTTON_LEFTSHOULDER => InputKey.Xbox_LB,
            SDL_GameControllerButton.SDL_CONTROLLER_BUTTON_RIGHTSHOULDER => InputKey.Xbox_RB,
            SDL_GameControllerButton.SDL_CONTROLLER_BUTTON_LEFTSTICK => InputKey.Xbox_LS_Click,
            SDL_GameControllerButton.SDL_CONTROLLER_BUTTON_RIGHTSTICK => InputKey.Xbox_RS_Click,
            SDL_GameControllerButton.SDL_CONTROLLER_BUTTON_DPAD_UP => InputKey.Xbox_DPadUp,
            SDL_GameControllerButton.SDL_CONTROLLER_BUTTON_DPAD_DOWN => InputKey.Xbox_DPadDown,
            SDL_GameControllerButton.SDL_CONTROLLER_BUTTON_DPAD_LEFT => InputKey.Xbox_DPadLeft,
            SDL_GameControllerButton.SDL_CONTROLLER_BUTTON_DPAD_RIGHT => InputKey.Xbox_DPadRight,
            _ => throw new ArgumentOutOfRangeException(nameof(btnEvent), $"Controller button not mapped: {btnEvent}")
        };
    }

    public static InputKey? FromControllerAxis(SDL_ControllerAxisEvent axisEvent) {
        const short threshold = 16000;

        return axisEvent.axis switch {
            (byte)SDL_GameControllerAxis.SDL_CONTROLLER_AXIS_LEFTX =>
                axisEvent.axisValue < -threshold ? InputKey.Xbox_LS_Left :
                axisEvent.axisValue > threshold ? InputKey.Xbox_LS_Right : null,
            (byte)SDL_GameControllerAxis.SDL_CONTROLLER_AXIS_LEFTY =>
                axisEvent.axisValue < -threshold ? InputKey.Xbox_LS_Up :
                axisEvent.axisValue > threshold ? InputKey.Xbox_LS_Down : null,
            (byte)SDL_GameControllerAxis.SDL_CONTROLLER_AXIS_RIGHTX =>
                axisEvent.axisValue < -threshold ? InputKey.Xbox_RS_Left :
                axisEvent.axisValue > threshold ? InputKey.Xbox_RS_Right : null,
            (byte)SDL_GameControllerAxis.SDL_CONTROLLER_AXIS_RIGHTY =>
                axisEvent.axisValue < -threshold ? InputKey.Xbox_RS_Up :
                axisEvent.axisValue > threshold ? InputKey.Xbox_RS_Down : null,
            (byte)SDL_GameControllerAxis.SDL_CONTROLLER_AXIS_TRIGGERLEFT =>
                axisEvent.axisValue > 0 ? InputKey.Xbox_LT : null,
            (byte)SDL_GameControllerAxis.SDL_CONTROLLER_AXIS_TRIGGERRIGHT =>
                axisEvent.axisValue > 0 ? InputKey.Xbox_RT : null,
            _ => null
        };
    }
}

public enum Color {
    White,
    Black,
    Gray,
    LightGray,
    DarkGray,
    Red,
    DarkRed,
    Green,
    DarkGreen,
    Blue,
    DarkBlue,
    Yellow,
    Orange,
    Cyan,
    Magenta,
    Purple,
    Pink,
    Brown,
    Lime,
    Teal,
    Navy,
    Olive,
    Maroon,
    Violet,
    Indigo,
    Gold,
    Silver
}

public static class ColorConverter {
    public static (byte r, byte g, byte b, byte a) ToRGBA(this Color color) => color switch {
        Color.White => (255, 255, 255, 255),
        Color.Black => (0, 0, 0, 255),
        Color.Gray => (128, 128, 128, 255),
        Color.LightGray => (200, 200, 200, 255),
        Color.DarkGray => (64, 64, 64, 255),
        Color.Red => (255, 0, 0, 255),
        Color.DarkRed => (139, 0, 0, 255),
        Color.Green => (0, 255, 0, 255),
        Color.DarkGreen => (0, 100, 0, 255),
        Color.Blue => (0, 0, 255, 255),
        Color.DarkBlue => (0, 0, 139, 255),
        Color.Yellow => (255, 255, 0, 255),
        Color.Orange => (255, 165, 0, 255),
        Color.Cyan => (0, 255, 255, 255),
        Color.Magenta => (255, 0, 255, 255),
        Color.Purple => (128, 0, 128, 255),
        Color.Pink => (255, 192, 203, 255),
        Color.Brown => (165, 42, 42, 255),
        Color.Lime => (0, 255, 0, 255),
        Color.Teal => (0, 128, 128, 255),
        Color.Navy => (0, 0, 128, 255),
        Color.Olive => (128, 128, 0, 255),
        Color.Maroon => (128, 0, 0, 255),
        Color.Violet => (238, 130, 238, 255),
        Color.Indigo => (75, 0, 130, 255),
        Color.Gold => (255, 215, 0, 255),
        Color.Silver => (192, 192, 192, 255),
        _ => (255, 255, 255, 255)
    };

    public static SDL.SDL_Color ToSDLColor(this Color color) {
        var (r, g, b, a) = color.ToRGBA();
        return new SDL.SDL_Color { r = r, g = g, b = b, a = a };
    }

    public static string ToHex(this Color color, bool includeAlpha = false) {
        var (r, g, b, a) = color.ToRGBA();
        return includeAlpha ? $"#{r:X2}{g:X2}{b:X2}{a:X2}" : $"#{r:X2}{g:X2}{b:X2}";
    }

    public static Color Lerp(this Color start, Color end, float t) {
        t = Math.Clamp(t, 0f, 1f);
        var (r1, g1, b1, a1) = start.ToRGBA();
        var (r2, g2, b2, a2) = end.ToRGBA();
        byte r = (byte)(r1 + (r2 - r1) * t);
        byte g = (byte)(g1 + (g2 - g1) * t);
        byte b = (byte)(b1 + (b2 - b1) * t);
        byte a = (byte)(a1 + (a2 - a1) * t);

        return Enum.GetValues<Color>()
                   .OrderBy(c => ColorDistance(c.ToRGBA(), (r, g, b, a)))
                   .First();
    }

    private static int ColorDistance((byte r, byte g, byte b, byte a) c1, (byte r, byte g, byte b, byte a) c2) {
        return Math.Abs(c1.r - c2.r) + Math.Abs(c1.g - c2.g) + Math.Abs(c1.b - c2.b);
    }

    public static Color Invert(this Color color) {
        var (r, g, b, a) = color.ToRGBA();
        byte invR = (byte)(255 - r);
        byte invG = (byte)(255 - g);
        byte invB = (byte)(255 - b);
        return Enum.GetValues<Color>()
                   .OrderBy(c => ColorDistance(c.ToRGBA(), (invR, invG, invB, a)))
                   .First();
    }
}

public static class ActionMapper {
    public static Action FromInput(InputKey input) {
        return input switch {
            InputKey.Key_W or InputKey.Key_UpArrow or
            InputKey.Xbox_DPadUp or InputKey.Xbox_LS_Up or
            InputKey.PS_DPadUp or InputKey.PS_LS_Up or
            InputKey.Switch_DPadUp or InputKey.Switch_LS_Up => Action.MoveUp,

            InputKey.Key_S or InputKey.Key_DownArrow or
            InputKey.Xbox_DPadDown or InputKey.Xbox_LS_Down or
            InputKey.PS_DPadDown or InputKey.PS_LS_Down or
            InputKey.Switch_DPadDown or InputKey.Switch_LS_Down => Action.MoveDown,

            InputKey.Key_A or InputKey.Key_LeftArrow or
            InputKey.Xbox_DPadLeft or InputKey.Xbox_LS_Left or
            InputKey.PS_DPadLeft or InputKey.PS_LS_Left or
            InputKey.Switch_DPadLeft or InputKey.Switch_LS_Left => Action.MoveLeft,

            InputKey.Key_D or InputKey.Key_RightArrow or
            InputKey.Xbox_DPadRight or InputKey.Xbox_LS_Right or
            InputKey.PS_DPadRight or InputKey.PS_LS_Right or
            InputKey.Switch_DPadRight or InputKey.Switch_LS_Right => Action.MoveRight,

            InputKey.Key_Space => Action.Jump,
            InputKey.Key_F => Action.Attack,
            InputKey.Key_E => Action.Interact,
            InputKey.Key_R => Action.Reload,
            InputKey.Key_C => Action.Crouch,
            InputKey.Key_LeftShift or InputKey.Xbox_LB or InputKey.PS_L1 or InputKey.Switch_L => Action.Sprint,
            InputKey.Key_LeftCtrl or InputKey.Xbox_RB or InputKey.PS_R1 or InputKey.Switch_R => Action.Dash,
            InputKey.Key_Q or InputKey.Xbox_Y or InputKey.PS_Triangle or InputKey.Switch_Y => Action.Use,
            InputKey.Key_Escape or InputKey.Xbox_Back or InputKey.PS_Share or InputKey.Switch_Minus => Action.OpenMenu,
            InputKey.Key_P or InputKey.Xbox_Start or InputKey.PS_Options or InputKey.Switch_Plus => Action.Pause,
            InputKey.Key_Enter or InputKey.Xbox_A or InputKey.PS_Cross or InputKey.Switch_A => Action.Confirm,
            InputKey.Key_Backspace or InputKey.Xbox_B or InputKey.PS_Circle or InputKey.Switch_B => Action.Cancel,

            _ => Action.None
        };
    }
}

public enum Action {
    None,
    MoveUp,
    MoveDown,
    MoveLeft,
    MoveRight,
    Jump,
    Attack,
    Interact,
    Reload,
    Crouch,
    Sprint,
    Dash,
    Use,
    OpenMenu,
    Pause,
    Confirm,
    Cancel
}

public enum InputKey {
    Key_A, Key_B, Key_C, Key_D, Key_E, Key_F, Key_G, Key_H, Key_I, Key_J,
    Key_K, Key_L, Key_M, Key_N, Key_O, Key_P, Key_Q, Key_R, Key_S, Key_T,
    Key_U, Key_V, Key_W, Key_X, Key_Y, Key_Z,
    Key_Num0, Key_Num1, Key_Num2, Key_Num3, Key_Num4, Key_Num5, Key_Num6, Key_Num7, Key_Num8, Key_Num9,
    Key_F1, Key_F2, Key_F3, Key_F4, Key_F5, Key_F6, Key_F7, Key_F8, Key_F9, Key_F10, Key_F11, Key_F12,
    Key_Shift, Key_Ctrl, Key_Alt, Key_CapsLock, Key_NumLock, Key_ScrollLock, Key_LeftShift, Key_RightShift,
    Key_LeftCtrl, Key_RightCtrl, Key_LeftAlt, Key_RightAlt, Key_LeftWindows, Key_RightWindows, Key_Menu,
    Key_UpArrow, Key_DownArrow, Key_LeftArrow, Key_RightArrow,
    Key_Home, Key_End, Key_PageUp, Key_PageDown, Key_Insert, Key_Delete,
    Key_Backspace, Key_Enter, Key_Tab, Key_Escape, Key_Space,
    Key_Tilde, Key_Minus, Key_Equals, Key_LeftBracket, Key_RightBracket, Key_Backslash,
    Key_Semicolon, Key_Quote, Key_Comma, Key_Period, Key_Slash,
    Key_NumPad0, Key_NumPad1, Key_NumPad2, Key_NumPad3, Key_NumPad4,
    Key_NumPad5, Key_NumPad6, Key_NumPad7, Key_NumPad8, Key_NumPad9,
    Key_NumPadDivide, Key_NumPadMultiply, Key_NumPadSubtract, Key_NumPadAdd, Key_NumPadDecimal, Key_NumPadEnter,
    Key_PrintScreen, Key_Pause, Key_Clear,

    Xbox_A, Xbox_B, Xbox_X, Xbox_Y,
    Xbox_LB, Xbox_RB,
    Xbox_LT, Xbox_RT,
    Xbox_Start, Xbox_Back,
    Xbox_LS_Click, Xbox_RS_Click,
    Xbox_DPadUp, Xbox_DPadDown, Xbox_DPadLeft, Xbox_DPadRight,
    Xbox_LS_Up, Xbox_LS_Down, Xbox_LS_Left, Xbox_LS_Right,
    Xbox_RS_Up, Xbox_RS_Down, Xbox_RS_Left, Xbox_RS_Right,

    PS_Cross, PS_Circle, PS_Square, PS_Triangle,
    PS_L1, PS_R1,
    PS_L2, PS_R2,
    PS_Options, PS_Share,
    PS_LS_Click, PS_RS_Click,
    PS_DPadUp, PS_DPadDown, PS_DPadLeft, PS_DPadRight,
    PS_LS_Up, PS_LS_Down, PS_LS_Left, PS_LS_Right,
    PS_RS_Up, PS_RS_Down, PS_RS_Left, PS_RS_Right,

    Switch_A, Switch_B, Switch_X, Switch_Y,
    Switch_L, Switch_R,
    Switch_ZL, Switch_ZR,
    Switch_Plus, Switch_Minus,
    Switch_LS_Click, Switch_RS_Click,
    Switch_DPadUp, Switch_DPadDown, Switch_DPadLeft, Switch_DPadRight,
    Switch_LS_Up, Switch_LS_Down, Switch_LS_Left, Switch_LS_Right,
    Switch_RS_Up, Switch_RS_Down, Switch_RS_Left, Switch_RS_Right
}


public abstract class Entity {
    public string Name { get; init; }
    public Guid Id { get; init; } = Guid.NewGuid();

    protected Entity(string name) => Name = name;
}

public class Player : Entity {
    private Player(string name) : base(name) { }

    public static Player Create(string name) => new Player(name);
}

class EntityStore {
    private readonly List<Entity> _store;

    public IReadOnlyList<Entity> Store => _store.ToList();

    public EntityStore(List<Entity>? initialStore = null) => _store = initialStore ?? new List<Entity>();

    public void Register(Entity entity) => _store.Add(entity);
}

interface IVector2 {
    Double X { get; }
    Double Y { get; }

    IVector2 WithXY(Double x, Double y);
}

class Vector2 : IVector2 {
    public Double X { get; private set; }
    public Double Y { get; private set; }

    private Vector2(Double x, Double y) => (X, Y) = (x, y);

    public static IVector2 Create(Double x, Double y) => new Vector2(x, y);

    public IVector2 WithXY(Double x, Double y) => Vector2.Create(x, y);
}

class PositionService {
    private readonly Dictionary<Guid, IVector2> _positionById;
    public event Action<Entity, double, double> OnTeleported;
    public event Action<Entity, double, double> OnMoved;

    public PositionService() => _positionById = new Dictionary<Guid, IVector2>();

    public void Register(Entity entity, IVector2 position) => _positionById[entity.Id] = position;

    public bool HasPosition(Entity entity) => _positionById.TryGetValue(entity.Id, out var value);
    public Double GetX(Entity entity) { _positionById.TryGetValue(entity.Id, out var value); return value.X; }
    public Double GetY(Entity entity) { _positionById.TryGetValue(entity.Id, out var value); return value.Y; }
    public (Double X, Double Y) GetPosition(Entity entity) => (GetX(entity), GetY(entity));

    public void Move(Entity entity, Double dX, Double dY) {
        if (_positionById.TryGetValue(entity.Id, out var position))
            _positionById[entity.Id] = position.WithXY((position.X + dX), (position.Y + dY));
        OnMoved?.Invoke(entity, _positionById[entity.Id].X, _positionById[entity.Id].Y);
    }

    public void Teleport(Entity entity, Double X, Double Y) {
        if (_positionById.TryGetValue(entity.Id, out var position))
            _positionById[entity.Id] = position.WithXY(X, Y);
        OnTeleported?.Invoke(entity, _positionById[entity.Id].X, _positionById[entity.Id].Y);
    }

    //possiblement recevoir le event du collider et décider si l'objet bouge ou il reste au meme endroit
}

interface IHealth {
    Int32 CurrentHealth { get; }
    Int32 MaxHealth { get; }

    IHealth WithCurrent(Int32 currentHealth);
    IHealth WithMax(Int32 maxHealth);
}

class Health : IHealth {
    public Int32 MaxHealth { get; private set; }
    public Int32 CurrentHealth { get; private set; }

    private Health(Int32 baseHealth = 100, Int32 maxHealth = 100) => (CurrentHealth, MaxHealth) = (baseHealth, maxHealth);

    public static Health Create(Int32 baseHealth = 100, Int32 maxHealth = 100) => new Health(baseHealth, maxHealth);

    public IHealth WithCurrent(Int32 currentHealth) => Health.Create(Math.Clamp(currentHealth, 0, MaxHealth), MaxHealth);
    public IHealth WithMax(Int32 maxHealth) => Health.Create(CurrentHealth, maxHealth);
}

class HealthService {
    private readonly Dictionary<Guid, IHealth> _healthById;
    public event Action<Entity, Int32, Int32, Int32> OnDamaged;
    public event Action<Entity, Int32, Int32, Int32> OnHealed;

    public HealthService() => _healthById = new Dictionary<Guid, IHealth>();

    public void Register(Entity entity, IHealth health) => _healthById[entity.Id] = health;

    public bool HasHealth(Entity entity) => _healthById.TryGetValue(entity.Id, out var value);
    public Int32 GetHealth(Entity entity) { _healthById.TryGetValue(entity.Id, out var value); return value.CurrentHealth; }
    public Int32 GetMaxHealth(Entity entity) { _healthById.TryGetValue(entity.Id, out var value); return value.MaxHealth; }

    public void Damage(Entity entity, Int32 damage) {
        if (_healthById.TryGetValue(entity.Id, out var health))
            _healthById[entity.Id] = health.WithCurrent(health.CurrentHealth - damage);
        OnDamaged?.Invoke(entity, damage, _healthById[entity.Id].CurrentHealth, _healthById[entity.Id].MaxHealth);
    }

    public void Heal(Entity entity, Int32 heal) {
        if (_healthById.TryGetValue(entity.Id, out var health))
            _healthById[entity.Id] = health.WithCurrent(health.CurrentHealth + heal);
        OnHealed?.Invoke(entity, heal, _healthById[entity.Id].CurrentHealth, _healthById[entity.Id].MaxHealth);
    }
}

class CollisionManager {
    public event Action<Entity, Entity> OnCollision;
    private readonly PositionService _positionService;
    private readonly EntityStore _entityStore;

    private const double EPSILON = 0.001;

    public CollisionManager(EntityStore entityStore, PositionService positionService) {
        (_entityStore, _positionService) = (entityStore, positionService);
        positionService.OnMoved += CheckCollisions;
    } 

    public void CheckCollisions(Entity entity, double x, double y) {
        foreach (var storedEntity in _entityStore.Store) {
            if (entity == storedEntity
                || !_positionService.HasPosition(storedEntity)
                || !_positionService.HasPosition(entity))
                continue;

            var posA = _positionService.GetPosition(entity);
            var posB = _positionService.GetPosition(storedEntity);

            if (Math.Abs(posA.X - posB.X) < EPSILON && (Math.Abs(posA.Y - posB.Y) < EPSILON))
                ResolveCollisions(entity, storedEntity);
        }
    }

    public void ResolveCollisions(Entity collider, Entity collided) => OnCollision?.Invoke(collider, collided);
}

public class ConversationManager {
    public event Action<Entity, Entity, string> OnConversationStarted;

    public void StartConversation(Entity a, Entity b, string message) {
        OnConversationStarted?.Invoke(a, b, message);
    }
}

abstract class InputDevice {
    public static event Action<InputDevice, InputKey> GlobalInput;
    public Guid Id { get; init; } = Guid.NewGuid();
    public string Name { get; set; }
    

    protected InputDevice(string name = "unknown_input_device") => Name = name;

    protected void TriggerInput(InputKey key) => GlobalInput?.Invoke(this, key);

    public abstract void ReadInput(SDL_Event e);
}

class Controller : InputDevice {
    public Controller(string name = "unknown_controller") : base(name) { }
    public override void ReadInput(SDL_Event e) {
        if (e.type == SDL_EventType.SDL_KEYDOWN || e.type == SDL_EventType.SDL_KEYUP) { }
        SDL_GameControllerButton button = (SDL_GameControllerButton)e.cbutton.button;
        InputKey mapped = InputKeyAdapterSDL.FromControllerButton(button);

        if (e.type == SDL_EventType.SDL_KEYDOWN) TriggerInput(mapped);
        else TriggerInput(mapped);
    }
}

class Keyboard : InputDevice {
    public Keyboard(string name = "unknown_keyboard") : base(name) { }
    public override void ReadInput(SDL_Event e) {
        if (e.type == SDL_EventType.SDL_KEYDOWN || e.type == SDL_EventType.SDL_KEYUP) { }
        SDL_Keycode keyCode = e.key.keysym.sym;
        InputKey mapped = InputKeyAdapterSDL.FromSDLKey(keyCode);

        if (e.type == SDL_EventType.SDL_KEYDOWN) TriggerInput(mapped);
        else TriggerInput(mapped);
    }
}

class InputService {
    public event Action<Entity, InputKey> EntityInputRequested;
    private readonly Dictionary<Guid, InputDevice> _deviceById;
    private readonly EntityStore _entityStore;

    public InputService(EntityStore entityStore) => (_entityStore, _deviceById) = (entityStore, new Dictionary<Guid, InputDevice>());

    public void BindInputDevice(InputDevice inputDevice, Entity entity) {
        _deviceById[entity.Id] = inputDevice; Int32 occurrences = 0;
        foreach (var device in _deviceById.Values)
            if (device.Name.LastIndexOf("_") >= 0 
                && device.Name.Substring(0, device.Name.LastIndexOf("_"))
                == inputDevice.Name) occurrences++;
        inputDevice.Name = inputDevice.Name + "_" + occurrences;
        _deviceById[entity.Id] = inputDevice;
    }

    // onRemove -> renommer les contro
    // leurs keyboard_2 -> keyboard_1

    public void ProcessInput(InputDevice inputDevice, InputKey inputKey) {
        foreach (var storedEntity in _entityStore.Store) {
            if (_deviceById.TryGetValue(storedEntity.Id, out var device) && device == inputDevice)
                ResolveInput(storedEntity, inputKey);
        }
    }

    public void ResolveInput(Entity entity, InputKey inputKey) => EntityInputRequested?.Invoke(entity, inputKey);
}

class InputManager {
    private readonly PositionService _positionService;
    private readonly InputService _inputService;
    private readonly EntityStore _entityStore;

    public InputManager(EntityStore entityStore, InputService inputService, PositionService positionService, ActionDispatcher actionDispatcher) {
        (_entityStore, _inputService, _positionService) = (entityStore, inputService, positionService);
        actionDispatcher.EntityActionRequested += MockMovePlayer;
    }

    public void MockMovePlayer(Entity entity, Action action) {
        switch (action) { // Juste pour tester
            case Action.MoveUp:
                _positionService.Move(entity, 0, -10);
                break;
            case Action.MoveLeft:
                _positionService.Move(entity, -10, 0);
                break;
            case Action.MoveDown:
                _positionService.Move(entity, 0, 10);
                break;
            case Action.MoveRight:
                _positionService.Move(entity, 10, 0);
                break;
            case Action.Use:
                Console.WriteLine($"{entity.Name} is using something mysterious...");
                break;

            default:
                //Console.WriteLine("Unmapped key!");
                break;
        }
    }
}

class ActionService {
    private readonly Dictionary<Guid, List<Action>> _actionsById;

    public ActionService() => _actionsById = new Dictionary<Guid, List<Action>>();

    public void Register(Entity entity, List<Action> actions) => _actionsById[entity.Id] = actions;

    public bool CanPerform(Entity entity, Action action) => _actionsById.ContainsKey(entity.Id) && _actionsById[entity.Id].Contains(action);

}

class ActionDispatcher {
    public event Action<Entity, Action> EntityActionRequested;
    private readonly ActionService _actionService;
    private bool _isMappingEnabled;

    public ActionDispatcher(InputService inputService, ActionService actionService) {
        inputService.EntityInputRequested += MapInput;
        _actionService = actionService;
    }

    // je pense que d'utiliser une nouvelel disposition genre
    // je pense que d'utiliser une nouvelel disposition genre
    // je pense que d'utiliser une nouvelel disposition genre
    // swap pour une disposition d'actions de menu serait mieux
    // swap pour une disposition d'actions de menu serait mieux
    // swap pour une disposition d'actions de menu serait mieux

    // FAUDRAIT CREER DES DIFFÉRENTES DISPOSITIONS
    
    // public void SetKeyMap() ou whatever...

    public bool GetInputMappingState() => _isMappingEnabled;
    public void ToggleInputMapping() => _isMappingEnabled = !_isMappingEnabled;
    public void EnableInputMapping() => _isMappingEnabled = true;
    public void DisableInputMapping() => _isMappingEnabled = false;

    public void MapInput(Entity entity, InputKey input) {
        var action = ActionMapper.FromInput(input);
        if (_actionService.CanPerform(entity, action) && _isMappingEnabled) ResolveMapping(entity, action);
        else {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine($"{entity.Name} is not allowed to perform '{action}'");
            Console.ResetColor();
        }
    }

    public void ResolveMapping(Entity entity, Action action) => EntityActionRequested?.Invoke(entity, action);

}

class ControllerService /*ou autre*/ {

}

class EntityControlService {
    //private Entity _controlled;
    // une map de entity <=> controller 
    //private readonly Dictionary<Guid, InputListener> _healthById;



    //une fonction register controller a quelque pars

    //on input event donc input manager ou whatever
    // onkeyboardkeypressed ou oncontrollerkeypressed events
    // quand ca trigger, apeller cette methode pour traiter l'input
    // probablement avoir a faire un store ou map d'entité controller

    //En fait, envoyer le input method et étant donné que chaque entité a un input method
    // différente de mappé a sa value, faire l'action ou event (pas bouger, c'est pas solid sinon)
    // mais genre juste créer un event qui dit
    // event on key... -> dans class qui crée un event
    // onkeypress event (trigger) -> on envoie l'instance du controlleur qui devrais etre bindé a un entité.
    // si controller1 est bindé a player1, la fonction crée/envoie des informations (event?) vers un manager
    // de input qui lui, effectue les bonnes opérations selon le key appuyé. donc peut etre une classe controller
    // et une classe mapper par type de controller.
}

interface ITexture { }
class Texture : ITexture {
    public IntPtr Handle { get; private set; }
    public SDL.SDL_Rect TextureRect { get; set; }
    public int Width => TextureRect.w;
    public int Height => TextureRect.h;

    public Texture(IntPtr handle, SDL.SDL_Rect rect) => (Handle, TextureRect) = (handle, rect);
}

interface IHitbox {

    int Width { get; }
    int Height { get; }
    int X { get; }
    int Y { get; }
}
class Hitbox : IHitbox {
    public SDL_Rect Box { get; set; }

    public int Width => Box.w;
    public int Height => Box.h;
    public int X => Box.x;
    public int Y => Box.y;

    public Hitbox(int x, int y, int w, int h) => Box = new SDL_Rect { x = x, y = y, w = w, h = h };
}

class TextureService {
    private readonly Dictionary<Guid, ITexture> _textureById;
    public event Action<Entity, ITexture> OnTextureUpdated;

    public TextureService() => _textureById = new Dictionary<Guid, ITexture>();

    public void Register(Entity entity, ITexture texture) {
        _textureById[entity.Id] = texture; OnTextureUpdated?.Invoke(entity, texture);
    } // A REVOIR, LOADER AVEC PATH

    public bool HasTexture(Entity entity) => _textureById.TryGetValue(entity.Id, out var value);
    public ITexture GetTexture(Entity entity) { _textureById.TryGetValue(entity.Id, out var value); return value; }

    public void ApplyTexture(Entity entity, ITexture texture) => Register(entity, texture);
    
    public void RemoveTexture(Entity entity) {
        if (_textureById.TryGetValue(entity.Id, out var texture)) _textureById[entity.Id] = null;
    }
}

class HitboxService {
    private readonly Dictionary<Guid, IHitbox> _hitboxById;
    public event Action<Entity, IHitbox> OnHitboxUpdated;

    public HitboxService() => _hitboxById = new Dictionary<Guid, IHitbox>();
    public void Register(Entity entity, IHitbox hitbox) {
        _hitboxById[entity.Id] = hitbox; OnHitboxUpdated?.Invoke(entity, hitbox);
    }

    public bool HasHitbox(Entity entity) => _hitboxById.TryGetValue(entity.Id, out var value);
    public IHitbox GetHitbox(Entity entity) { _hitboxById.TryGetValue(entity.Id, out var value); return value; }

    public void SetHitbox(Entity entity, IHitbox hitbox) => Register(entity, hitbox);

    public void DeleteHitbox(Entity entity) {
        if (_hitboxById.TryGetValue(entity.Id, out var hitbox)) _hitboxById[entity.Id] = null;
    }
}

class SpriteManager {
    private readonly PositionService _positionService;
    private readonly TextureService _textureService;
    private readonly HitboxService _hitBoxService;
    

    /* PAS CERTAIN FINALEMENT */

    //getTexture
    //

    
    
}

class RenderSystem {
    private readonly IntPtr _renderer;

    public RenderSystem(IntPtr renderer) => _renderer = renderer;

    public void RenderPresent() => SDL.SDL_RenderPresent(_renderer);
    public void RenderClear() => SDL.SDL_RenderClear(_renderer);

    public void SetRenderColor(byte r, byte g, byte b, byte a) => SDL_SetRenderDrawColor(_renderer, r, g, b, a);
    public void SetRenderColor(Color color) {
        var (r, g, b, a) = color.ToRGBA(); SetRenderColor(r, g, b, a);
    }

    public void DrawRectangle(ref SDL_Rect rect, Color color = Color.White) => DrawRectangle(rect.x, rect.y, rect.w, rect.h, color);
    public void DrawRectangle(Int32 x, Int32 y, Int32 w, Int32 h, Color color = Color.White) {
        SetRenderColor(color); SDL_Rect rect = new SDL_Rect { x = x, y = y, w = w, h = h };
        SDL_RenderDrawRect(_renderer, ref rect);
    }

    public void FillRectangle(ref SDL_Rect rect, Color color = Color.White) => DrawRectangle(rect.x, rect.y, rect.w, rect.h, color);
    public void FillRectangle(Int32 x, Int32 y, Int32 w, Int32 h, Color color = Color.White) {
        SetRenderColor(color); SDL_Rect rect = new SDL_Rect { x = x, y = y, w = w, h = h };
        SDL_RenderFillRect(_renderer, ref rect);
    }

    public void DrawLine(Int32 x1, Int32 y1, Int32 x2, Int32 y2, Color color = Color.White) {
        SetRenderColor(color); SDL.SDL_RenderDrawLine(_renderer, x1, y1, x2, y2);
    }

    public void DrawPoint(Int32 x, Int32 y, Color color = Color.White) {
        SetRenderColor(color); SDL.SDL_RenderDrawPoint(_renderer, x, y);
    }

    public void RenderCopy(IntPtr texture, nint zero, SDL_Rect dstRect) =>
        SDL.SDL_RenderCopy(_renderer, texture, IntPtr.Zero, ref dstRect);
    public void RenderCopy(IntPtr texture, SDL_Rect srcRect, SDL_Rect dstRect) =>
        SDL.SDL_RenderCopy(_renderer, texture, ref srcRect, ref dstRect);
   
    public void SetTextureColorMod(IntPtr texture, byte r, byte g, byte b) => SDL.SDL_SetTextureColorMod(texture, r, g, b);
    public void SetTextureColorMod(IntPtr texture, Color color = Color.White) {
        var (r, g, b, a) = color.ToRGBA(); SetTextureColorMod(texture, r, g, b);
    }

    public void SetTextureAlphaMod(IntPtr texture, byte a) => SDL.SDL_SetTextureAlphaMod(texture, a);

    public void SetTextureBlendMode(IntPtr texture, SDL_BlendMode mode) => SDL.SDL_SetTextureBlendMode(texture, SDL.SDL_BlendMode.SDL_BLENDMODE_BLEND);
}

class RenderService {
    private readonly RenderSystem _renderSystem;

    public RenderService() { }

    //probablement faire un sprite service ou une shit qui me permet de binder un set collision sprite a un entity
    //il faut aussi réfléchit au fait qu'il y aura des spritesheet plus tard donc type générique ou abstrait?????

    public void ReadInput(Entity entity) { } // ou a quelque pars dautre genre input manager ou whatever, c pas render
}

class Sprite { // À DELETER
    public IntPtr Texture { get; private set; }
    public SDL_Rect SourceRect { get; set; }
    public SDL_Rect DestRect { get; set; }

    public Sprite(IntPtr texture, SDL_Rect source, SDL_Rect dest) {
        Texture = texture;
        SourceRect = source;
        DestRect = dest;
    }
}

class Program {
    static void Main(string[] args) {
        // TODO: faire un GameService/Manager qui contient un instance du EntityStore et qui injecte...

        List<Action> playerActions = new List<Action> {
            Action.MoveUp,
            Action.MoveDown,
            Action.MoveLeft,
            Action.MoveRight,
            Action.Jump,
            Action.Attack,
            Action.Dash,
            Action.OpenMenu,
            Action.Cancel,
            Action.Use
        };

        var globalStore = new EntityStore();

        var healthService = new HealthService();
        var positionService = new PositionService();
        var collisionManager = new CollisionManager(globalStore, positionService);

        var textureService = new TextureService();
        var hitboxService = new HitboxService();

        var inputService = new InputService(globalStore);

        var actionService = new ActionService();
        var actionDispatcher = new ActionDispatcher(inputService, actionService);

        var InputManager = new InputManager(globalStore, inputService, positionService, actionDispatcher);

        var manager = new ConversationManager();

        /* FAIRE UN LOGGER QUI EST SUB A TOUS LES EVENTS */

        manager.OnConversationStarted += (a, b, message) => {
            Console.WriteLine($"[Dialogue] {a.Name} → {b.Name}: \"{message}\"");
        };

        positionService.OnMoved += (entity, x, y) => {
            Console.WriteLine($"[Movement] {entity.Name} moved to ({x:0.##}, {y:0.##})");
            //collisionManager.CheckCollisions(entity);
        };

        positionService.OnTeleported += (entity, x, y) => {
            Console.WriteLine($"[Movement] {entity.Name} teleported to ({x:0.##}, {y:0.##})");
        };

        healthService.OnDamaged += (entity, damage, currentHealth, maxHealth) => {
            Console.WriteLine($"[Health] {entity.Name} took {damage} damage | HP: {currentHealth}/{maxHealth}");
        };

        healthService.OnHealed += (entity, heal, currentHealth, maxHealth) => {
            Console.WriteLine($"[Health] {entity.Name} healed by {heal} | HP: {currentHealth}/{maxHealth}");
        };

        collisionManager.OnCollision += (collider, collided) => {
            Console.ForegroundColor = ConsoleColor.Magenta;
            Console.WriteLine($"[Collision] {collider.Name} collided with {collided.Name}");
            Console.ResetColor();
        };

        inputService.EntityInputRequested += (entity, key) => {
            Console.WriteLine($"[Input] {entity.Name} pressed {key.ToString()}");
        };

        actionDispatcher.EntityActionRequested += (entity, action) => {
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine($"[Input] {entity.Name} executed {action}");
            Console.ResetColor();
        };



        Entity player1 = Player.Create("Alpha");
        Entity player2 = Player.Create("Bravo");
        Entity player3 = Player.Create("Charlie");
        Entity player4 = Player.Create("Delta");
        Entity player5 = Player.Create("Echo");

        /* onInput event, apelle la inputService.ProcessInput() */

        globalStore.Register(player1);
        globalStore.Register(player2);
        globalStore.Register(player3);
        globalStore.Register(player4);
        globalStore.Register(player5);

        InputDevice controller1 = new Controller();
        InputDevice keyboard1 = new Keyboard();
        InputDevice keyboard2 = new Keyboard();

        InputDevice.GlobalInput += (inputDevice, key) => {
            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine($"[Input] Device '{inputDevice.Name}' triggered '{key.ToString()}'");
            Console.ResetColor();
            inputService.ProcessInput(inputDevice, key);
        };

        inputService.BindInputDevice(controller1, player1);
        inputService.BindInputDevice(keyboard1, player2);
        inputService.BindInputDevice(keyboard2, player3);

        //controller1.ReadInput();
        //controller1.ReadInput();
        //controller1.ReadInput();
        //controller1.ReadInput();
        //controller1.ReadInput();
        //keyboard1.ReadInput();

        actionService.Register(player2, playerActions);

        //Console.WriteLine(actionService.CanPerform(player1, Actions.OpenMenu));



        //inputService.ProcessInput(controller1, new ControllerInputKey(ControllerButtons.A));
        //inputService.ProcessInput(controller1, new ControllerInputKey(ControllerButtons.B));
        //inputService.ProcessInput(keyboard1, new KeyboardInputKey(KeyboardKeys.Space));


        manager.StartConversation(player1, player2, "d");

        IHealth basicHealth = Health.Create(100, 100);
        IVector2 startPos = Vector2.Create(0, 0);

        healthService.Register(player1, basicHealth);
        healthService.Register(player2, basicHealth);
        healthService.Register(player3, basicHealth);

        positionService.Register(player1, Vector2.Create(0, 0));
        positionService.Register(player2, Vector2.Create(0, 0));
        positionService.Register(player3, Vector2.Create(0, 0));

        healthService.Damage(player1, 75);
        healthService.Heal(player1, 25);
        healthService.Damage(player1, 10);

        healthService.Damage(player2, 50);
        healthService.Heal(player2, 10);

        healthService.Damage(player3, 60);

        positionService.Move(player1, 100, 00);
        positionService.Move(player2, 50, 250);
        positionService.Move(player3, 500, 150);

        positionService.Teleport(player1, 10, 10);

        /*********************************************************/

        var entities = new List<List<object>>();
        foreach (var e in globalStore.Store) {
            int? currentHealth = healthService.HasHealth(e) ? healthService.GetHealth(e) : null;
            int? maxHealth = healthService.HasHealth(e) ? healthService.GetMaxHealth(e) : null;

            var pos = positionService.HasPosition(e)
                ? positionService.GetPosition(e)
                : (Double.NaN, Double.NaN);

            var strHealth
                = (currentHealth is null || maxHealth is null)
                ? "Invalid"
                : $"{currentHealth}/{maxHealth}";

            entities.Add(new List<object> {
                e.Name,
                strHealth,
                $"({pos.Item1:0.##}, {pos.Item2:0.##})"
            });
        }

        ConsoleTableBuilder
            .From(entities)
            .WithColumn(new List<string> { "Entity", "Health", "Position" })
            .WithCharMapDefinition(ConsoleTableExt.CharMapDefinition.FramePipDefinition)
            .ExportAndWriteLine();


        //while (true) {
        //    keyboard1.ReadInput();
        //}

        /*========================================================================================*/


        SDL_Init(SDL_INIT_VIDEO | SDL_INIT_JOYSTICK | SDL_INIT_GAMECONTROLLER);
        IntPtr window = SDL_CreateWindow("Quark Engine", SDL_WINDOWPOS_CENTERED, SDL_WINDOWPOS_CENTERED,
                                         1600, 900, SDL_WindowFlags.SDL_WINDOW_RESIZABLE);
        IntPtr renderer = SDL_CreateRenderer(window, -1, SDL_RendererFlags.SDL_RENDERER_ACCELERATED | SDL_RendererFlags.SDL_RENDERER_PRESENTVSYNC);

        var renderSystem = new RenderSystem(renderer);

        IntPtr surfacePtr = SDL_image.IMG_Load("assets/sprites/player.png");
        if (surfacePtr == IntPtr.Zero) {
            Console.WriteLine("Error loading the image: " + SDL_image.IMG_GetError());
            return;
        }

        IntPtr playerTexture = SDL.SDL_CreateTextureFromSurface(renderer, surfacePtr);
        SDL.SDL_SetTextureBlendMode(playerTexture, SDL.SDL_BlendMode.SDL_BLENDMODE_BLEND);
        SDL.SDL_FreeSurface(surfacePtr);

        var spriteRect = new SDL.SDL_Rect { w = 64, h = 64 };
        var playerTex = new Texture(playerTexture, spriteRect);
        textureService.Register(player2, playerTex);

        var playerHitbox = new Hitbox(0, 0, 64, 64);
        hitboxService.Register(player2, playerHitbox);

        bool quit = false;
        bool menu = false;

        while (!quit) {
            while (SDL_PollEvent(out var e) != 0) {
                if (e.type == SDL_EventType.SDL_QUIT) quit = true;
                if (e.type == SDL_EventType.SDL_KEYDOWN) {
                    if (e.key.keysym.sym == SDL.SDL_Keycode.SDLK_ESCAPE) {
                        actionDispatcher.DisableInputMapping(); menu = !menu;
                    } keyboard1.ReadInput(e); // mettre dans le render service avec la liste de inputservice++
                                             // readInput(entity) -> aller voir dans la classe pour notes...
                }
            }

            renderSystem.SetRenderColor(Color.Black);
            renderSystem.RenderClear();

            foreach (var entity in new[] { player2 }) {
                if (textureService.HasTexture(entity) && positionService.HasPosition(entity)) {
                    var tex = (Texture)textureService.GetTexture(entity);
                    double x = positionService.GetX(entity);
                    double y = positionService.GetY(entity);

                    SDL.SDL_Rect dstRect = new SDL.SDL_Rect { x = (int)x, y = (int)y, w = tex.Width, h = tex.Height };
                    renderSystem.RenderCopy(tex.Handle, IntPtr.Zero, dstRect);
                }

                if (hitboxService.HasHitbox(entity)) { // la hitbox ne suit pas le joueur, figure out plus tard...
                    var hb = hitboxService.GetHitbox(entity);
                    SDL.SDL_Rect hbRect = new SDL.SDL_Rect { x = hb.X, y = hb.Y, w = hb.Width, h = hb.Height };
                    renderSystem.DrawRectangle(ref hbRect, Color.Red);
                }
            }

            int screenWidth, screenHeight;
            SDL_GetWindowSize(window, out screenWidth, out screenHeight);
            if (menu) { // faire une classe CréateurDeMenues() ou whatever qui créer des menus sur mesure
                int rectWidth = screenWidth / 2, rectHeight = screenHeight / 2;

                SDL.SDL_Rect rectOuter = new SDL.SDL_Rect {
                    x = screenWidth / 2 - rectWidth / 2,
                    y = screenHeight / 2 - rectHeight / 2,
                    w = rectWidth,
                    h = rectHeight
                };

                SDL.SDL_Rect rectInner = new SDL.SDL_Rect {
                    x = screenWidth / 2 - (rectWidth - 10) / 2,
                    y = screenHeight / 2 - (rectHeight - 10) / 2,
                    w = rectWidth - 10,
                    h = rectHeight - 10
                };

                renderSystem.DrawRectangle(ref rectOuter, Color.White);
                renderSystem.DrawRectangle(ref rectInner, Color.White);
            } else if (actionDispatcher.GetInputMappingState() == false)
                    actionDispatcher.EnableInputMapping();

            // je pense que d'utiliser une nouvelel disposition genre
            // swap pour une disposition d'actions de menu serait mieux


            renderSystem.RenderPresent();
        }

        SDL.SDL_DestroyTexture(playerTexture);
        SDL.SDL_DestroyRenderer(renderer);
        SDL.SDL_DestroyWindow(window);
        SDL.SDL_Quit();

    }
}
```

## Latest Progress
<img width="1602" height="932" alt="image" src="https://github.com/user-attachments/assets/ff4ce1c7-6e44-4fc3-8f82-69860559ca5b" />
Rendering a sprite offset from its hitbox by 10 units. Modularity++

## Output
```console
[Input] Device 'Undefined Controller' triggered 'Start'
[Input] Alpha pressed Start
[Input] Device 'Undefined Controller' triggered 'Start'
[Input] Alpha pressed Start
[Input] Device 'Undefined Keyboard' triggered 'Alt'
[Input] Bravo pressed Alt
[Input] Device 'Undefined Keyboard' triggered 'Alt'
[Input] Bravo pressed Alt
[Dialogue] Alpha ␦ Bravo: "d"
[Health] Alpha took 75 damage | HP: 25/100
[Health] Alpha healed by 25 | HP: 50/100
[Health] Alpha took 10 damage | HP: 40/100
[Health] Bravo took 50 damage | HP: 50/100
[Health] Bravo healed by 10 | HP: 60/100
[Health] Charlie took 60 damage | HP: 40/100
[Movement] Alpha moved to (10, 0)
[Collision] Bravo collided with Charlie
[Movement] Bravo moved to (0, 0)
[Movement] Alpha teleported to (10, 10)
┌─────────┬─────────┬────────────┐
│ Entity  │ Health  │ Position   │
├─────────┼─────────┼────────────┤
│ Alpha   │ 40/100  │ (10, 10)   │
├─────────┼─────────┼────────────┤
│ Bravo   │ 60/100  │ (0, 0)     │
├─────────┼─────────┼────────────┤
│ Charlie │ 40/100  │ (0, 0)     │
├─────────┼─────────┼────────────┤
│ Delta   │ Invalid │ (NaN, NaN) │
├─────────┼─────────┼────────────┤
│ Echo    │ Invalid │ (NaN, NaN) │
└─────────┴─────────┴────────────┘
```

## Entity Input Handling with Collision Support
```console
[Input] Bravo pressed Key_A
[Input] Device 'unknown_keyboard_0' triggered 'Key_A'
[Movement] Bravo moved to (30, 10)
[Input] Bravo executed MoveLeft
[Input] Bravo pressed Key_A
[Input] Device 'unknown_keyboard_0' triggered 'Key_A'
[Movement] Bravo moved to (20, 10)
[Input] Bravo executed MoveLeft
[Input] Bravo pressed Key_A
[Input] Device 'unknown_keyboard_0' triggered 'Key_A'
[Collision] Bravo collided with Alpha
[Movement] Bravo moved to (10, 10)
[Input] Bravo executed MoveLeft
[Input] Bravo pressed Key_A
[Input] Device 'unknown_keyboard_0' triggered 'Key_A'
[Movement] Bravo moved to (0, 10)
[Input] Bravo executed MoveLeft
[Input] Bravo pressed Key_A
[Input] Device 'unknown_keyboard_0' triggered 'Key_D'
[Collision] Bravo collided with Alpha
[Movement] Bravo moved to (10, 10)
[Input] Bravo executed MoveRight
[Input] Bravo pressed Key_D
[Input] Device 'unknown_keyboard_0' triggered 'Key_T'
Bravo is not allowed to perform 'None'
[Input] Bravo pressed Key_T
[Input] Device 'unknown_keyboard_0' triggered 'Key_E'
Bravo is not allowed to perform 'Interact'
[Input] Bravo pressed Key_E
```

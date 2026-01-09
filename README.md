class BabboNatale(arcade.Window):
    def __init__(self, larghezza, altezza, titolo):
        super().__init__(larghezza, altezza, titolo)
        self.babbo = None
        self.cookie = None
        self.lista_babbo = arcade.SpriteList()
        self.lista_cookie = arcade.SpriteList()
        self.suono_munch = arcade.load_sound(r"C:\Users\Utente\Desktop\Sebastiano Scarpa 1H\Informatica\python\babbo natale\munch.mp3")
        self.up_pressed = False
        self.down_pressed = False
        self.left_pressed = False
        self.right_pressed = False
        self.sfondo=None
        self.velocita = 4
        self.audio_attivo=True
        self.biscotti_raccolti = 0
        self.punteggio = 0
        self.setup()
    
    def setup(self):
        self.babbo = arcade.Sprite(r"C:\Users\Utente\Desktop\Sebastiano Scarpa 1H\Informatica\python\babbo natale\babbo.png")
        self.babbo.center_x = 300
        self.babbo.center_y = 100
        self.babbo.scale = 1.0
        self.lista_babbo.append(self.babbo)
        self.crea_cookie()
        self.sfondo=arcade.load_texture(r"C:\Users\Utente\Desktop\Sebastiano Scarpa 1H\Informatica\python\babbo natale\sfondo.png")
    def crea_cookie(self):
        self.cookie = arcade.Sprite(r"C:\Users\Utente\Desktop\Sebastiano Scarpa 1H\Informatica\python\babbo natale\cookie.png")
        probabilità = random.random()
        if probabilità < 0.03:
            self.cookie = arcade.Sprite(r"C:\Users\Utente\Desktop\Sebastiano Scarpa 1H\Informatica\python\babbo natale\biscotto d'oro.png")
            self.cookie.tipo = "oro"
        else:
            self.cookie = arcade.Sprite(r"C:\Users\Utente\Desktop\Sebastiano Scarpa 1H\Informatica\python\babbo natale\cookie.png")
            self.cookie.tipo = "normal"
        while True:
            nuovo_x=random.randint(50, 550)
            nuovo_y=random.randint(50, 550)
            diff_x = nuovo_x-self.babbo.center_x
            diff_y = nuovo_y-self.babbo.center_y
            if diff_x < 100:
                diff_x = -diff_x
            if diff_y < 100:
                diff_y = -diff_y
            if diff_x > 100 or diff_y > 100:
                break
        #self.cookie.center_x = random.randint(50, 550)
        #self.cookie.center_y = random.randint(50, 550)
        self.cookie.center_x = nuovo_x
        self.cookie.center_y = nuovo_y
        self.cookie.scale = 0.2
        self.lista_cookie.append(self.cookie)
    
    def on_draw(self):
        self.clear()
        arcade.draw_texture_rect(self.sfondo, arcade.LBWH(0, 0, self.width, self.height))
        self.lista_cookie.draw()
        self.lista_babbo.draw()
        arcade.draw_text(f"Biscotti: {self.biscotti_raccolti}", 20, self.height -40, arcade.color.WHITE, 20)
        arcade.draw_text(f"Punteggio: {self.punteggio}", 20, self.height -70, arcade.color.WHITE, 20)

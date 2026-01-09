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
    def on_update(self, delta_time):
        # Calcola movimento in base ai tasti premuti
        change_x = 0
        change_y = 0
        
        if self.up_pressed:
            change_y += self.velocita
        if self.down_pressed:
            change_y -= self.velocita
        if self.left_pressed:
            change_x -= self.velocita
        if self.right_pressed:
            change_x += self.velocita
        
        # Applica movimento
        self.babbo.center_x += change_x
        self.babbo.center_y += change_y
        
        # Flip orizzontale in base alla direzione
        if change_x < 0: 
            self.babbo.scale = (-1, 1)
        elif change_x > 0:
            self.babbo.scale = (1, 1)
        
        # Limita movimento dentro lo schermo
        if self.babbo.center_x < 0:
            self.babbo.center_x = 0
        elif self.babbo.center_x > self.width:
            self.babbo.center_x = self.width
        
        if self.babbo.center_y < 0:
            self.babbo.center_y = 0
        elif self.babbo.center_y > self.height:
            self.babbo.center_y = self.height
        
        # Gestione collisioni
        collisioni = arcade.check_for_collision_with_list(self.babbo, self.lista_cookie)

        if len(collisioni) > 0: # Vuol dire che il personaggio si è scontrato con qualcosa
            for cookie in collisioni:
                self.biscotti_raccolti += 1
                if cookie.tipo == "oro":
                    self.punteggio += 100
                else:
                    self.punteggio += 10
                if self.audio_attivo:
                    arcade.play_sound(self.suono_munch)
                for cookie in collisioni:
                    cookie.remove_from_sprite_lists()
                    self.crea_cookie() # creo un altro biscotto
                if self.biscotti_raccolti % 5 == 0:
                    self.crea_cookie()
    def on_key_press(self, tasto, modificatori):
        collisioni = arcade.check_for_collision_with_list(self.babbo, self.lista_cookie)
        if tasto in (arcade.key.UP, arcade.key.W):
            self.up_pressed = True
        elif tasto in (arcade.key.DOWN, arcade.key.S):
            self.down_pressed = True
        elif tasto in (arcade.key.LEFT, arcade.key.A):
            self.left_pressed = True
        elif tasto in (arcade.key.RIGHT, arcade.key.D):
            self.right_pressed = True
        if tasto == arcade.key.M:
            self.audio_attivo= not self.audio_attivo
            
    def on_key_release(self, tasto, modificatori):
        """Gestisce il rilascio dei tasti"""
        if tasto in (arcade.key.UP, arcade.key.W):
            self.up_pressed = False
        elif tasto in (arcade.key.DOWN, arcade.key.S):
            self.down_pressed = False
        elif tasto in (arcade.key.LEFT, arcade.key.A):
            self.left_pressed = False
        elif tasto in (arcade.key.RIGHT, arcade.key.D):
            self.right_pressed = False

def main():
    gioco = BabboNatale(600, 600, "Babbo Natale")
    arcade.run()


if __name__ == "__main__":
    main()

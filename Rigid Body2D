extends RigidBody2D

@export var vento_forca: float = 500.0 
@export var vento_forca_min: float = 150.0
@export var vento_forca_max: float = 2000.0
@export var vento_aceleracao: float = 400.0
@export var vento_desaceleracao: float = 800.0

@onready var vento_particles: GPUParticles2D = $GPUParticles2D
@onready var vento_particles_material: ParticleProcessMaterial = $GPUParticles2D.process_material

var direcao_gravidade: Vector2 = Vector2.DOWN
var potencia_gravidade: float = 980.0
var vento_direcao: Vector2
var coeficiente_arrasto: float = 0.47 
var densidade_ar: float = 0.1
var area_frontal: float = 0.02
var drag_on: bool = true
var vento_ativo: bool = false


func _ready() -> void:
	var tela_game_over = get_tree().get_first_node_in_group("tela_game_over")
	if tela_game_over:
		tela_game_over.pedra = self


func _process(delta: float) -> void:
	self.vento_particles.global_position = global_position

	# Controle de aceleração e desaceleração do vento
	if vento_ativo:
		vento_forca = clamp(vento_forca + vento_aceleracao * delta, vento_forca_min, vento_forca_max)
	else:
		vento_forca = clamp(vento_forca - vento_desaceleracao * delta, vento_forca_min, vento_forca_max)


func _integrate_forces(state: PhysicsDirectBodyState2D) -> void:
	var vetor_gravidade = direcao_gravidade.normalized() * potencia_gravidade
	apply_central_force(vetor_gravidade * mass)

	var vento_normal_direcao = vento_direcao.normalized() 
	var vento_aplicado = vento_normal_direcao * vento_forca 
	apply_central_force(vento_aplicado)

	var vel = linear_velocity
	var speed = vel.length()
	if drag_on and speed > 0.01:
		var drag_magnitude = 0.5 * densidade_ar * coeficiente_arrasto * area_frontal * speed * speed
		var drag_force = -vel.normalized() * drag_magnitude
		apply_central_force(drag_force)


func _input(event: InputEvent) -> void:
	if event.is_action_pressed("ui_up"):
		gravidade_cima()
	elif event.is_action_pressed("ui_down"):
		gravidade_baixo()
	elif event.is_action_pressed("ui_left"):
		gravidade_esquerda()
	elif event.is_action_pressed("ui_right"):
		gravidade_direita()
	elif event.is_action_pressed("PararArrasto"):
		alternar_arrasto()

	# --- Vento contínuo com aceleração ---
	if event.is_action_pressed("vento_cima"):
		set_vento_direcao(Vector2.UP)
		vento_particles_material.gravity = Vector3(0, -potencia_gravidade, 0)
		vento_ativo = true
	elif event.is_action_pressed("vento_baixo"):
		set_vento_direcao(Vector2.DOWN)
		vento_particles_material.gravity = Vector3(0, potencia_gravidade, 0)
		vento_ativo = true
	elif event.is_action_pressed("vento_esquerda"):
		set_vento_direcao(Vector2.LEFT)
		vento_particles_material.gravity = Vector3(-potencia_gravidade, 0, 0)
		vento_ativo = true
	elif event.is_action_pressed("vento_direita"):
		set_vento_direcao(Vector2.RIGHT)
		vento_particles_material.gravity = Vector3(potencia_gravidade, 0, 0)
		vento_ativo = true

	# Quando soltar qualquer tecla de vento, vento desacelera
	if event.is_action_released("vento_cima") \
	or event.is_action_released("vento_baixo") \
	or event.is_action_released("vento_esquerda") \
	or event.is_action_released("vento_direita"):
		vento_ativo = false


func resetar(posicao: Vector2):
	linear_velocity = Vector2.ZERO
	angular_velocity = 0
	direcao_gravidade = Vector2.DOWN
	vento_direcao = Vector2.DOWN
	global_position = posicao
	drag_on = true


func set_gravidade_direcao(direcao: Vector2) -> void:
	direcao_gravidade = direcao.normalized()


func set_vento_direcao (direcao: Vector2) ->void:
	vento_direcao = direcao.normalized() 
	var rosa = get_tree().get_first_node_in_group("rosa_vento")
	if rosa:
		rosa.atualizar_direcao(vento_direcao)


func alternar_arrasto():
	drag_on = not drag_on


func gravidade_baixo() -> void:
	set_gravidade_direcao(Vector2.DOWN)


func gravidade_cima() -> void:
	set_gravidade_direcao(Vector2.UP)


func gravidade_esquerda() -> void:
	set_gravidade_direcao(Vector2.LEFT)


func gravidade_direita() -> void:
	set_gravidade_direcao(Vector2.RIGHT)


func vento_baixo () -> void: 
	set_vento_direcao(Vector2.DOWN)
	vento_particles_material.gravity = Vector3(0, potencia_gravidade, 0)


func vento_cima ()-> void: 
	set_vento_direcao(Vector2.UP)
	vento_particles_material.gravity = Vector3(0, -potencia_gravidade, 0)


func vento_esquerda() ->void:
	set_vento_direcao(Vector2.LEFT)
	vento_particles_material.gravity = Vector3(-potencia_gravidade, 0, 0)


func vento_direita() ->void: 
	set_vento_direcao(Vector2.RIGHT)
	vento_particles_material.gravity = Vector3(potencia_gravidade, 0, 0)



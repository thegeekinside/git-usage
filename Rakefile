# encoding: UTF-8

require 'git'
$workspaces_base = '/Users/thegeekinside/Source/ruby/spikes/'
$workspace       = 'git-usage'
$workspace_path = File.join($workspaces_base, $workspace)


desc "Etiqueta el release"
task :TagRelease, [:fullVersion,:isRelease] do |t, args|
	isRelease = (args[:isRelease] || "true").to_b

	CalculateVersionByInstance(args[:fullVersion], "medtzin") do |major, minor, build, bugfix|
		revision = isRelease ? 0 : bugfix
		release = "release-#{major}.#{minor.to_i}.#{build}.#{isRelease ? 0 : bugfix}"
		tag_release(release)
	end
end

def HazAlgoConEsteRelease(release)
	g = Git.open($workspace_path)
	g.log.each do |c|
		p c.message
	end

	g.remotes.each do |r|
		p r.name
	end

	g.add_tag(release)
	g.repack
	g.push(g.remote("origin"), release)
	puts "El número de release es: #{release}"
end

# Se agrega este comentario para prueba
def tag_release(release)
	g = Git.open($workspace_path)

	g.add_tag(release)
	g.repack
	g.push(g.remote("origin"), release)
end

#-- Legacy
$APP_VERSION_PRODUCTO_MAYOR              = '5'
$APP_VERSION_PRODUCTO_MENOR              = '0105'

$INSTANCIAS = {
	:medtzin => 1,
	:ssh     => 2,
	:issemym => 3
}

def CalculateVersionByInstance(version, instancia)
		instancia = instancia.downcase.to_sym  # convertimos la cadena a un symbolo
		ValidateInstance(instancia)

		/^([0-9]{1,5})\.([a-zA-Z0-9]{2})(\d{2})\.([0-9]{1,5})\.([0-9]{1,5})$/ =~ version
		major, instance, minor_instance, build, bugfix = $1, $2, $3, $4, $5

		instance = sprintf '%02d', $INSTANCIAS[instancia]
		minor = "#{instance}#{minor_instance}"

		yield major, minor, build, bugfix
end

def ValidateInstance(instance)
	if !$INSTANCIAS.has_key?(instance.to_sym)
		abort("Numero de instancia incorrecto")
	end
end

#-- Utils
class String
	def to_b
		case self.downcase.strip
		when 'true', 'yes', 'on', 't', '1', 'y', '=='
			return true
		when 'nil', 'null'
			return nil
		else
			return false
		end
	end
end
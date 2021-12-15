# testes_software
Trabalho Prático sobre Testes de Software

Vitor Ribeiro dos Santos - 2017023676

Rocket.Chat(https://github.com/RocketChat/Rocket.Chat)

Rocket.Chat é uma plataforma de comunicações de código aberto totalmente customizável desenvolvida em JavaScript para organizações com altos padrões de proteção de dados.

Vamos mostrar tres exemplos de teste de software na plataforma Rocket.Chat

# Exemplo 1

Rocket.Chat implementa um método de sendSimpleMessage que envia uma única mensagem para a sala que o cliente está inserido.

Nesse método temos o seguinte teste:

export const sendSimpleMessage = ({ roomId, text = 'test message', tmid }) => {
	if (!roomId) {
		throw new Error('"roomId" is required in "sendSimpleMessage" test helper');
	}
	const message = {
		rid: roomId,
		text,
	};
	if (tmid) {
		message.tmid = tmid;
	}

	return request.post(api('chat.sendMessage'))
		.set(credentials)
		.send({ message });
};

No teste o metodo recebe o id da sala, a mensagem e o contexto e espera-se que ele retorne um mensagem de erro caso não tenha passado as informações corretas ou sucesso caso consiga enviar a mensagem


# Exemplo 2

Rocket.Chat tem um método para criar emojis com base em uma nova imagem que o cliente envie para API

Nesse metodo temos o seguinte teste

it('should create new custom emoji', (done) => {
			request.post(api('emoji-custom.create'))
				.set(credentials)
				.attach('emoji', imgURL)
				.field({
					name: customEmojiName,
					aliases: `${ customEmojiName }-alias`,
				})
				.expect('Content-Type', 'application/json')
				.expect(200)
				.expect((res) => {
					expect(res.body).to.have.property('success', true);
				})
				.end(done);
		});
    
Nesse método ele aciona o metodo create de emoji-custom enviando a imagem e espera que ele retorna sucesso ou erro

# Exemplo 3

Rocket.chat tem um método para retonar a lista de sons customizados que um cliente deseja utilzar no chat

Nesse metodo temos o seguinte teste:

describe('[/custom-sounds.list]', () => {
		it('should return custom sounds', (done) => {
			request.get(api('custom-sounds.list'))
				.set(credentials)
				.expect(200)
				.expect((res) => {
					expect(res.body).to.have.property('sounds').and.to.be.an('array');
					expect(res.body).to.have.property('total');
					expect(res.body).to.have.property('offset');
					expect(res.body).to.have.property('count');
				})
				.end(done);
		});
	});
  
Nesse método ele aciona o método list de custom-sounds aguaradnno o retorno da lista de sons a ser utilizado

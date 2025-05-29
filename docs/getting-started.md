import React, { useState } from 'react';

// Define as cores da PMMG e tons neutros para o novo design
const PMMG_COLORS = {
  blue: '#003399', // Azul-blau da PMMG
  red: '#A02020',  // Vermelho Rubro (mais profundo)
  gold: '#FFCC00', // Amarelo-ouro da PMMG
  lightBlueBg: '#F8FBFD', // Fundo muito claro, quase branco
  darkText: '#2C3E50', // Azul escuro para textos principais (similar ao LinkedIn)
  mediumText: '#7F8C8D', // Cinza médio para textos secundários
  lightBorder: '#ECF0F1', // Cinza muito claro para bordas e divisores
  white: '#FFFFFF',
  accentBlue: '#3498DB', // Um azul mais vibrante para detalhes (opcional, se necessário)
};

// Configuração do Tailwind CSS para cores personalizadas e estilos globais
const tailwindConfig = `
  <style>
    :root {
      --pmmg-blue: ${PMMG_COLORS.blue};
      --pmmg-red: ${PMMG_COLORS.red};
      --pmmg-gold: ${PMMG_COLORS.gold};
      --pmmg-light-blue-bg: ${PMMG_COLORS.lightBlueBg};
      --pmmg-dark-text: ${PMMG_COLORS.darkText};
      --pmmg-medium-text: ${PMMG_COLORS.mediumText};
      --pmmg-light-border: ${PMMG_COLORS.lightBorder};
      --pmmg-white: ${PMMG_COLORS.white};
    }
    .bg-pmmg-blue { background-color: var(--pmmg-blue); }
    .text-pmmg-blue { color: var(--pmmg-blue); }
    .border-pmmg-blue { border-color: var(--pmmg-blue); }

    .bg-pmmg-red { background-color: var(--pmmg-red); }
    .text-pmmg-red { color: var(--pmmg-red); }
    .border-pmmg-red { border-color: var(--pmmg-red); }

    .bg-pmmg-gold { background-color: var(--pmmg-gold); }
    .text-pmmg-gold { color: var(--pmmg-gold); }
    .border-pmmg-gold { border-color: var(--pmmg-gold); }

    .bg-pmmg-light-blue-bg { background-color: var(--pmmg-light-blue-bg); }
    .text-pmmg-dark-text { color: var(--pmmg-dark-text); }
    .text-pmmg-medium-text { color: var(--pmmg-medium-text); }
    .border-pmmg-light-border { border-color: var(--pmmg-light-border); }

    body { font-family: 'Inter', sans-serif; }

    /* Estilos para cards, com sombras mais suaves e bordas arredondadas */
    .card-modern {
      background-color: var(--pmmg-white);
      border-radius: 1rem; /* Mais arredondado */
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08); /* Sombra mais suave */
      border: 1px solid var(--pmmg-light-border);
      padding: 1.5rem;
    }

    /* Estilos globais para botões */
    button {
      transition: all 0.2s ease-in-out;
      border-radius: 0.75rem; /* rounded-xl */
      font-weight: 600; /* semibold */
      padding: 1rem 1.5rem; /* py-4 px-6 */
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Sombra padrão para botões */
    }
    button:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
    }
    button:focus {
      outline: none;
      box-shadow: 0 0 0 4px rgba(0, 51, 153, 0.2); /* Sombra de foco azul */
    }

    /* Estilos globais para inputs */
    input[type="text"], input[type="email"] {
      border-radius: 0.5rem; /* rounded-lg */
      padding: 0.85rem 1.15rem;
      border: 1px solid var(--pmmg-light-border);
      transition: all 0.2s ease-in-out;
      color: var(--pmmg-dark-text);
      background-color: var(--pmmg-light-blue-bg); /* Fundo suave para inputs */
    }
    input[type="text"]:focus, input[type="email"]:focus {
      outline: none;
      border-color: var(--pmmg-blue);
      box-shadow: 0 0 0 3px rgba(0, 51, 153, 0.15);
      background-color: var(--pmmg-white);
    }
    .text-input-label {
        color: var(--pmmg-dark-text);
        font-weight: 500; /* medium */
        margin-bottom: 0.5rem;
    }

    /* Estilos para mensagens de feedback */
    .alert-success {
      background-color: #D4EDDA; /* Verde claro */
      border-color: #28A745; /* Verde escuro */
      color: #155724; /* Texto verde */
    }
    .alert-error {
      background-color: #F8D7DA; /* Vermelho claro */
      border-color: #DC3545; /* Vermelho escuro */
      color: #721C24; /* Texto vermelho */
    }
  </style>
`;

// Dados simulados para demonstração
const mockMototaxistas = [
  { id: 'moto1', nome: 'João da Silva', placa: 'ABC-1234', selo: true },
  { id: 'moto2', nome: 'Maria Oliveira', placa: 'DEF-5678', selo: true },
  { id: 'moto3', nome: 'Carlos Souza', placa: 'GHI-9012', selo: false },
];

const mockEstabelecimentos = [
  { id: 'est1', nome: 'Pizzaria Sabor', endereco: 'Rua A, 123', selo: true },
  { id: 'est2', nome: 'Restaurante Bom Prato', endereco: 'Av. B, 456', selo: true },
  { id: 'est3', nome: 'Farmácia Central', endereco: 'Rua C, 789', selo: false },
];

// Componente de Cabeçalho Reutilizável
const Header = ({ title, onBack }) => (
  <header className="bg-pmmg-blue text-white p-4 flex items-center justify-between shadow-xl rounded-b-xl">
    {onBack && (
      <button onClick={onBack} className="text-white text-2xl font-bold p-2 rounded-full hover:bg-pmmg-red transition-colors duration-300">
        &larr;
      </button>
    )}
    <h1 className="text-2xl font-bold flex-grow text-center">{title}</h1>
    {/* Representação do Logo da PMMG - Mais detalhada e moderna */}
    <div className="w-10 h-10 flex items-center justify-center">
      <svg viewBox="0 0 100 100" fill="currentColor" className="text-pmmg-gold">
        {/* Forma de escudo simplificada */}
        <path d="M50 5 L90 25 V75 L50 95 L10 75 V25 Z" fill="currentColor" stroke="white" strokeWidth="4"/>
        {/* Estrela no centro */}
        <polygon points="50,25 58,42 75,42 62,55 68,72 50,62 32,72 38,55 25,42 42,42" fill="white"/>
      </svg>
    </div>
  </header>
);

// Componente do Selo Oficial (SVG) - Cores ajustadas
const OfficialSeal = ({ size = 'w-6 h-6', className = '' }) => (
  <svg className={`${size} ${className}`} viewBox="0 0 100 100" fill="none" xmlns="http://www.w3.org/2000/svg">
    <circle cx="50" cy="50" r="45" fill={PMMG_COLORS.gold} stroke={PMMG_COLORS.blue} strokeWidth="5"/>
    <path d="M30 50 L45 65 L70 35" stroke="white" strokeWidth="8" strokeLinecap="round" strokeLinejoin="round"/>
    <text x="50" y="50" fontFamily="Inter, sans-serif" fontSize="20" fill={PMMG_COLORS.blue} textAnchor="middle" alignmentBaseline="middle" fontWeight="bold">
      SELO
    </text>
    <text x="50" y="75" fontFamily="Inter, sans-serif" fontSize="12" fill={PMMG_COLORS.blue} textAnchor="middle" alignmentBaseline="middle" fontWeight="bold">
      OFICIAL
    </text>
  </svg>
);

// Página de Login/Seleção de Perfil
const LoginPage = ({ setUserType }) => (
  <div className="flex flex-col items-center justify-center h-full p-6 bg-gradient-to-br from-pmmg-light-blue-bg to-pmmg-white rounded-xl shadow-inner">
    <h2 className="text-pmmg-blue text-4xl font-extrabold mb-8 text-center leading-tight">
      Acesse o <br />Avança Poços
    </h2>
    <p className="text-pmmg-dark-text text-lg text-center mb-12 max-w-md">
      Escolha seu perfil para continuar:
    </p>
    <div className="flex flex-col space-y-6 w-full max-w-sm">
      <button
        onClick={() => setUserType('pm')}
        className="bg-pmmg-red text-white text-xl font-semibold py-4 px-6 rounded-xl shadow-lg hover:bg-pmmg-blue transition-all duration-300 transform hover:scale-105 focus:outline-none focus:ring-4 focus:ring-pmmg-red focus:ring-opacity-50"
      >
        Sou Policial Militar
      </button>
      <button
        onClick={() => setUserType('mototaxista')}
        className="bg-pmmg-gold text-pmmg-blue text-xl font-semibold py-4 px-6 rounded-xl shadow-lg hover:bg-yellow-600 transition-all duration-300 transform hover:scale-105 focus:outline-none focus:ring-4 focus:ring-pmmg-gold focus:ring-opacity-50"
      >
        Sou Mototaxista
      </button>
      <button
        onClick={() => setUserType('estabelecimento')}
        className="bg-pmmg-blue text-white text-xl font-semibold py-4 px-6 rounded-xl shadow-lg hover:bg-pmmg-red transition-all duration-300 transform hover:scale-105 focus:outline-none focus:ring-4 focus:ring-pmmg-blue focus:ring-opacity-50"
      >
        Sou Estabelecimento
      </button>
    </div>
  </div>
);

// Dashboard do PM
const PmDashboard = ({ setCurrentPage }) => {
  const [searchType, setSearchType] = useState(null); // 'mototaxista' or 'estabelecimento'
  const [searchTerm, setSearchTerm] = useState('');
  const [searchResults, setSearchResults] = useState([]);
  const [selectedChat, setSelectedChat] = useState(null); // ID do chat selecionado (simulado)
  const [currentChatMessages, setCurrentChatMessages] = useState([]);
  const [chatInput, setChatInput] = useState('');

  // Simulação de chats recebidos para o PM
  const [inboxChats, setInboxChats] = useState([
    { id: 'chat1', from: 'Mototaxista João', subject: 'Dúvida sobre Selo Oficial', messages: [{ sender: 'Mototaxista João', text: 'Bom dia, Sargento! Tenho uma dúvida sobre como fixar o selo na minha moto.' }] },
    { id: 'chat2', from: 'Estabelecimento Pizzaria Sabor', subject: 'Parceria com a campanha', messages: [{ sender: 'Estabelecimento Pizzaria Sabor', text: 'Olá PM, gostaríamos de entender melhor como podemos aderir à campanha e exibir o selo.' }] },
  ]);

  const handleSearch = () => {
    setSearchResults([]); // Limpa resultados anteriores
    if (searchTerm.trim() === '') {
      alert('Por favor, digite um termo para buscar.');
      return;
    }
    if (searchType === 'mototaxista') {
      const results = mockMototaxistas.filter(m =>
        m.nome.toLowerCase().includes(searchTerm.toLowerCase()) ||
        m.placa.toLowerCase().includes(searchTerm.toLowerCase())
      );
      setSearchResults(results);
    } else if (searchType === 'estabelecimento') {
      const results = mockEstabelecimentos.filter(e =>
        e.nome.toLowerCase().includes(searchTerm.toLowerCase()) ||
        e.endereco.toLowerCase().includes(searchTerm.toLowerCase())
      );
      setSearchResults(results);
    }
  };

  const handleSelectChat = (chatId) => {
    const chat = inboxChats.find(c => c.id === chatId);
    setSelectedChat(chatId);
    setCurrentChatMessages(chat ? chat.messages : []);
  };

  const handleSendMessage = () => {
    if (chatInput.trim() === '') return;
    const newMessage = { sender: 'PM', text: chatInput };
    setCurrentChatMessages(prev => [...prev, newMessage]);
    setChatInput('');

    // Simular resposta do outro lado
    setTimeout(() => {
      const responseMessage = { sender: selectedChat.from, text: 'Obrigado pela sua mensagem! Já estamos analisando.' };
      setCurrentChatMessages(prev => [...prev, responseMessage]);
    }, 1500);
  };

  return (
    <div className="flex flex-col h-full p-6 bg-pmmg-light-blue-bg rounded-lg shadow-inner overflow-y-auto">
      <h2 className="text-pmmg-blue text-3xl font-bold mb-6 text-center">Área do Policial Militar</h2>

      {/* Seção de Fiscalização */}
      <div className="card-modern mb-6">
        <h3 className="text-pmmg-red font-bold text-xl mb-4">Fiscalização</h3>
        <div className="flex flex-col sm:flex-row space-y-3 sm:space-y-0 sm:space-x-4 mb-4">
          <button
            onClick={() => setSearchType('mototaxista')}
            className={`flex-1 py-3 px-4 rounded-lg font-semibold transition-colors ${searchType === 'mototaxista' ? 'bg-pmmg-blue text-white' : 'bg-gray-200 text-pmmg-dark-text hover:bg-gray-300'}`}
          >
            Fiscalizar Mototaxista
          </button>
          <button
            onClick={() => setSearchType('estabelecimento')}
            className={`flex-1 py-3 px-4 rounded-lg font-semibold transition-colors ${searchType === 'estabelecimento' ? 'bg-pmmg-blue text-white' : 'bg-gray-200 text-pmmg-dark-text hover:bg-gray-300'}`}
          >
            Fiscalizar Estabelecimento
          </button>
        </div>

        {searchType && (
          <div className="mt-4">
            <input
              type="text"
              placeholder={`Buscar por nome ou ${searchType === 'mototaxista' ? 'placa' : 'endereço'}...`}
              value={searchTerm}
              onChange={(e) => setSearchTerm(e.target.value)}
              className="w-full p-3 border-pmmg-light-border rounded-lg mb-4 focus:outline-none focus:ring-2 focus:ring-pmmg-blue"
            />
            <button
              onClick={handleSearch}
              className="bg-pmmg-blue text-white py-3 px-6 rounded-xl font-semibold w-full hover:bg-pmmg-red transition-all duration-300"
            >
              Buscar
            </button>

            {searchResults.length > 0 && (
              <div className="mt-4 space-y-3">
                <h4 className="font-bold text-pmmg-dark-text">Resultados:</h4>
                {searchResults.map((item) => (
                  <div key={item.id} className="bg-gray-50 p-4 rounded-lg border border-pmmg-light-border flex flex-col sm:flex-row items-center justify-between">
                    <div className="text-center sm:text-left mb-2 sm:mb-0">
                      <p className="font-semibold text-pmmg-dark-text">{item.nome}</p>
                      {item.placa && <p className="text-sm text-pmmg-medium-text">Placa: {item.placa}</p>}
                      {item.endereco && <p className="text-sm text-pmmg-medium-text">Endereço: {item.endereco}</p>}
                    </div>
                    {item.selo ? (
                      <div className="flex items-center text-pmmg-gold">
                        <OfficialSeal size="w-8 h-8" className="mr-2" />
                        <span className="text-pmmg-blue font-bold">Participante!</span>
                      </div>
                    ) : (
                      <span className="text-pmmg-red font-bold">Não Participante</span>
                    )}
                  </div>
                ))}
              </div>
            )}
            {searchTerm && searchResults.length === 0 && (
                <p className="mt-4 text-pmmg-medium-text text-center">Nenhum resultado encontrado para "{searchTerm}".</p>
            )}
          </div>
        )}
      </div>

      {/* Seção de Esclarecimento de Dúvidas/Inbox */}
      <div className="card-modern">
        <h3 className="text-pmmg-red font-bold text-xl mb-4">Caixa de Entrada (Dúvidas e Conversas)</h3>
        <div className="flex flex-col md:flex-row h-96">
          {/* Lista de Chats */}
          <div className="w-full md:w-1/3 border-r border-pmmg-light-border pr-4 md:mb-0 mb-4 overflow-y-auto">
            <h4 className="font-bold text-pmmg-dark-text mb-3">Conversas Recebidas:</h4>
            {inboxChats.length === 0 ? (
              <p className="text-pmmg-medium-text text-sm">Nenhuma mensagem nova.</p>
            ) : (
              inboxChats.map(chat => (
                <div
                  key={chat.id}
                  onClick={() => handleSelectChat(chat.id)}
                  className={`p-3 mb-2 rounded-lg cursor-pointer transition-colors duration-200 ${selectedChat === chat.id ? 'bg-pmmg-light-blue-bg border-l-4 border-pmmg-blue' : 'bg-gray-50 hover:bg-gray-100'}`}
                >
                  <p className="font-semibold text-pmmg-blue">{chat.from}</p>
                  <p className="text-sm text-pmmg-dark-text truncate">{chat.subject}</p>
                </div>
              ))
            )}
          </div>

          {/* Área de Conversa */}
          <div className="w-full md:w-2/3 md:pl-4 flex flex-col">
            {selectedChat ? (
              <>
                <h4 className="font-bold text-pmmg-dark-text mb-3">Conversa com {inboxChats.find(c => c.id === selectedChat).from}</h4>
                <div className="border border-pmmg-light-border rounded-lg p-4 flex-grow flex flex-col overflow-y-auto bg-gray-50">
                  <div className="flex-grow space-y-2">
                    {currentChatMessages.map((msg, index) => (
                      <div key={index} className={`p-2 rounded-lg max-w-[80%] ${msg.sender === 'PM' ? 'bg-pmmg-blue text-white self-end ml-auto' : 'bg-gray-300 text-pmmg-dark-text self-start mr-auto'}`}>
                        <span className="font-bold">{msg.sender}: </span>{msg.text}
                      </div>
                    ))}
                  </div>
                </div>
                <div className="mt-4 flex">
                  <input
                    type="text"
                    placeholder={`Digite sua mensagem para ${inboxChats.find(c => c.id === selectedChat).from}...`}
                    value={chatInput}
                    onChange={(e) => setChatInput(e.target.value)}
                    className="flex-grow p-2 border-pmmg-light-border rounded-l-lg focus:outline-none focus:ring-2 focus:ring-pmmg-blue"
                  />
                  <button
                    onClick={handleSendMessage}
                    className="bg-pmmg-blue text-white py-2 px-4 rounded-r-lg hover:bg-pmmg-red transition-colors"
                  >
                    Enviar
                  </button>
                </div>
              </>
            ) : (
              <p className="text-center text-pmmg-medium-text mt-20">Selecione uma conversa para visualizar.</p>
            )}
          </div>
        </div>
      </div>
    </div>
  );
};

// Dashboard do Mototaxista
const MototaxistaDashboard = ({ setCurrentPage }) => {
  const [searchEstabelecimentoTerm, setSearchEstabelecimentoTerm] = useState('');
  const [foundEstablishment, setFoundEstablishment] = useState(null);
  const [raffleMessage, setRaffleMessage] = useState('');
  const [supportMessages, setSupportMessages] = useState([]); // Adicionado para o suporte do mototaxista
  const [supportInput, setSupportInput] = useState(''); // Adicionado para o suporte do mototaxista

  const handleSearchEstabelecimento = () => {
    setFoundEstablishment(null); // Clear previous
    if (searchEstabelecimentoTerm.trim() === '') {
      alert('Por favor, digite um termo para buscar.');
      return;
    }
    const result = mockEstabelecimentos.find(e =>
      e.nome.toLowerCase().includes(searchEstabelecimentoTerm.toLowerCase()) ||
      e.endereco.toLowerCase().includes(searchEstabelecimentoTerm.toLowerCase())
    );
    if (result) {
      setFoundEstablishment(result);
    } else {
      setFoundEstablishment({ nome: 'Nenhum estabelecimento encontrado.', selo: false });
    }
  };

  const handleParticipateRaffle = () => {
    setRaffleMessage('Processando sua participação no sorteio...');
    setTimeout(() => {
      const success = Math.random() > 0.5; // Simulate success
      if (success) {
        setRaffleMessage(
          <div className="alert-success px-4 py-3 rounded relative" role="alert">
            <strong className="font-bold">Parabéns!</strong>
            <span className="block sm:inline ml-2">Sua participação no sorteio foi registrada com sucesso! Boa sorte!</span>
          </div>
        );
      } else {
        setRaffleMessage(
          <div className="alert-error px-4 py-3 rounded relative" role="alert">
            <strong className="font-bold">Ops!</strong>
            <span className="block sm:inline ml-2">Houve um erro ao registrar sua participação. Tente novamente mais tarde.</span>
          </div>
        );
      }
    }, 2000);
  };

  const handleSendSupportMessage = () => {
    if (supportInput.trim() === '') return;
    setSupportMessages([...supportMessages, { sender: 'Mototaxista', text: supportInput }]);
    setSupportInput('');
    // Simulate a support response
    setTimeout(() => {
      setSupportMessages(prev => [...prev, { sender: 'Suporte', text: 'Sua mensagem foi recebida. Em breve um de nossos atendentes entrará em contato.' }]);
    }, 1500);
  };

  return (
    <div className="flex flex-col h-full p-6 bg-pmmg-light-blue-bg rounded-lg shadow-inner overflow-y-auto">
      <h2 className="text-pmmg-blue text-3xl font-bold mb-6 text-center">Área do Mototaxista</h2>

      {/* Seção de Informações e Esclarecimentos */}
      <div className="card-modern mb-6">
        <h3 className="text-pmmg-blue font-bold text-xl mb-4">Informações e Dúvidas</h3>
        <button
          onClick={() => setCurrentPage('info')}
          className="bg-gray-200 text-pmmg-dark-text text-lg font-semibold py-3 px-6 rounded-xl shadow-md hover:bg-gray-300 transition-all duration-300 w-full focus:outline-none focus:ring-4 focus:ring-gray-400 focus:ring-opacity-50"
        >
          Entender a Campanha / Esclarecer Dúvidas
        </button>
      </div>

      {/* Seção de Sorteios */}
      <div className="card-modern mb-6">
        <h3 className="text-pmmg-blue font-bold text-xl mb-4">Participar de Sorteios</h3>
        <p className="text-pmmg-dark-text mb-4">
          Participe dos sorteios exclusivos para mototaxistas legalizados e concorra a prêmios incríveis!
        </p>
        <button
          onClick={handleParticipateRaffle}
          className="bg-pmmg-blue text-white text-lg font-semibold py-3 px-6 rounded-xl shadow-md hover:bg-pmmg-red transition-all duration-300 w-full focus:outline-none focus:ring-4 focus:ring-pmmg-blue focus:ring-opacity-50"
        >
          Participar do Sorteio Atual
        </button>
        {raffleMessage && <div className="mt-4 text-center">{raffleMessage}</div>}
      </div>

      {/* Seção de Verificação de Estabelecimento */}
      <div className="card-modern mb-6">
        <h3 className="text-pmmg-blue font-bold text-xl mb-4">Verificar Estabelecimento Parceiro</h3>
        <input
          type="text"
          placeholder="Nome ou endereço do estabelecimento..."
          value={searchEstabelecimentoTerm}
          onChange={(e) => setSearchEstabelecimentoTerm(e.target.value)}
          className="w-full p-3 border-pmmg-light-border rounded-lg mb-4 focus:outline-none focus:ring-2 focus:ring-pmmg-blue"
        />
        <button
          onClick={handleSearchEstabelecimento}
          className="bg-pmmg-blue text-white py-3 px-6 rounded-xl font-semibold w-full hover:bg-pmmg-red transition-all duration-300"
        >
          Buscar Estabelecimento
        </button>
        {foundEstablishment && (
          <div className="mt-4 bg-gray-50 p-4 rounded-lg border border-pmmg-light-border">
            <p className="font-semibold text-pmmg-dark-text">{foundEstablishment.nome}</p>
            {foundEstablishment.endereco && <p className="text-sm text-pmmg-medium-text">Endereço: {foundEstablishment.endereco}</p>}
            {foundEstablishment.selo ? (
              <div className="flex items-center text-pmmg-gold mt-2">
                <OfficialSeal size="w-8 h-8" className="mr-2" />
                <span className="text-pmmg-blue font-bold">Participante da Campanha!</span>
              </div>
            ) : (
              <span className="text-pmmg-red font-bold mt-2">Não aderiu à Campanha ainda.</span>
            )}
          </div>
        )}
        {searchEstabelecimentoTerm && !foundEstablishment && (
            <p className="mt-4 text-pmmg-medium-text text-center">Nenhum resultado encontrado para "{searchEstabelecimentoTerm}".</p>
        )}
      </div>

      {/* Seção de Suporte Específico para Mototaxista */}
      <div className="card-modern">
        <h3 className="text-pmmg-blue font-bold text-xl mb-4">Suporte e Auxílio</h3>
        <div className="border border-pmmg-light-border rounded-lg p-4 h-64 flex flex-col overflow-y-auto bg-gray-50">
          <div className="flex-grow space-y-2">
            {supportMessages.length === 0 && <p className="text-center text-pmmg-medium-text">Envie uma mensagem para nossa equipe de suporte.</p>}
            {supportMessages.map((msg, index) => (
              <div key={index} className={`p-2 rounded-lg max-w-[80%] ${msg.sender === 'Mototaxista' ? 'bg-pmmg-blue text-white self-end ml-auto' : 'bg-gray-300 text-pmmg-dark-text self-start mr-auto'}`}>
                <span className="font-bold">{msg.sender}: </span>{msg.text}
              </div>
            ))}
          </div>
          <div className="mt-4 flex">
            <input
              type="text"
              placeholder="Digite sua mensagem para o suporte..."
              value={supportInput}
              onChange={(e) => setSupportInput(e.target.value)}
              className="flex-grow p-2 border-pmmg-light-border rounded-l-lg focus:outline-none focus:ring-2 focus:ring-pmmg-blue"
            />
            <button
              onClick={handleSendSupportMessage}
              className="bg-pmmg-blue text-white py-2 px-4 rounded-r-lg hover:bg-pmmg-red transition-colors"
            >
              Enviar
            </button>
          </div>
        </div>
      </div>
    </div>
  );
};

// Dashboard do Estabelecimento
const EstabelecimentoDashboard = ({ setCurrentPage }) => {
  const [searchMototaxistaTerm, setSearchMototaxistaTerm] = useState('');
  const [foundMototaxista, setFoundMototaxista] = useState(null);
  const [supportMessages, setSupportMessages] = useState([]);
  const [supportInput, setSupportInput] = useState('');
  const [donationMessage, setDonationMessage] = useState(''); // Estado para mensagem de doação

  const handleSearchMototaxista = () => {
    setFoundMototaxista(null); // Clear previous
    if (searchMototaxistaTerm.trim() === '') {
      alert('Por favor, digite um termo para buscar.');
      return;
    }
    const result = mockMototaxistas.find(m =>
      m.nome.toLowerCase().includes(searchMototaxistaTerm.toLowerCase()) ||
      m.placa.toLowerCase().includes(searchMototaxistaTerm.toLowerCase())
    );
    if (result) {
      setFoundMototaxista(result);
    } else {
      setFoundMototaxista({ nome: 'Nenhum mototaxista encontrado.', selo: false });
    }
  };

  const handleSendSupportMessage = () => {
    if (supportInput.trim() === '') return;
    setSupportMessages([...supportMessages, { sender: 'Estabelecimento', text: supportInput }]);
    setSupportInput('');
    // Simulate a support response
    setTimeout(() => {
      setSupportMessages(prev => [...prev, { sender: 'Suporte', text: 'Sua mensagem foi recebida. Em breve um de nossos atendentes entrará em contato.' }]);
    }, 1500);
  };

  const handleDonateForRaffle = () => {
    setDonationMessage('Processando sua doação para o sorteio...');
    setTimeout(() => {
      const success = Math.random() > 0.5; // Simula sucesso/falha
      if (success) {
        setDonationMessage(
          <div className="alert-success px-4 py-3 rounded relative" role="alert">
            <strong className="font-bold">Obrigado!</strong>
            <span className="block sm:inline ml-2">Sua doação foi registrada com sucesso. Entraremos em contato para mais detalhes!</span>
          </div>
        );
      } else {
        setDonationMessage(
          <div className="alert-error px-4 py-3 rounded relative" role="alert">
            <strong className="font-bold">Ops!</strong>
            <span className="block sm:inline ml-2">Houve um erro ao registrar sua doação. Por favor, tente novamente.</span>
          </div>
        );
      }
    }, 2000);
  };

  return (
    <div className="flex flex-col h-full p-6 bg-pmmg-light-blue-bg rounded-lg shadow-inner overflow-y-auto">
      <h2 className="text-pmmg-blue text-3xl font-bold mb-6 text-center">Área do Estabelecimento</h2>

      {/* Seção de Informações e Esclarecimentos */}
      <div className="card-modern mb-6">
        <h3 className="text-pmmg-dark-text font-bold text-xl mb-4">Informações e Dúvidas</h3>
        <button
          onClick={() => setCurrentPage('info')}
          className="bg-gray-200 text-pmmg-dark-text text-lg font-semibold py-3 px-6 rounded-xl shadow-md hover:bg-gray-300 transition-all duration-300 w-full focus:outline-none focus:ring-4 focus:ring-gray-400 focus:ring-opacity-50"
        >
          Entender a Campanha / Esclarecer Dúvidas
        </button>
      </div>

      {/* Seção de Doar para Sorteio */}
      <div className="card-modern mb-6">
        <h3 className="text-pmmg-blue font-bold text-xl mb-4">Doar para Sorteio</h3>
        <p className="text-pmmg-dark-text mb-4">
          Contribua com a campanha doando prêmios para os sorteios dos mototaxistas legalizados. Sua participação incentiva a formalização e a segurança!
        </p>
        <button
          onClick={handleDonateForRaffle}
          className="bg-pmmg-gold text-pmmg-blue text-lg font-semibold py-3 px-6 rounded-xl shadow-md hover:bg-yellow-600 transition-all duration-300 w-full focus:outline-none focus:ring-4 focus:ring-pmmg-gold focus:ring-opacity-50"
        >
          Oferecer Doação
        </button>
        {donationMessage && <div className="mt-4 text-center">{donationMessage}</div>}
      </div>

      {/* Seção de Verificação de Mototaxista */}
      <div className="card-modern mb-6">
        <h3 className="text-pmmg-dark-text font-bold text-xl mb-4">Verificar Mototaxista Contratado</h3>
        <input
          type="text"
          placeholder="Nome ou placa do mototaxista..."
          value={searchMototaxistaTerm}
          onChange={(e) => setSearchMototaxistaTerm(e.target.value)}
          className="w-full p-3 border-pmmg-light-border rounded-lg mb-4 focus:outline-none focus:ring-2 focus:ring-pmmg-blue"
        />
        <button
          onClick={handleSearchMototaxista}
          className="bg-pmmg-blue text-white py-3 px-6 rounded-xl font-semibold w-full hover:bg-pmmg-red transition-all duration-300"
        >
          Buscar Mototaxista
        </button>
        {foundMototaxista && (
          <div className="mt-4 bg-gray-50 p-4 rounded-lg border border-pmmg-light-border">
            <p className="font-semibold text-pmmg-dark-text">{foundMototaxista.nome}</p>
            {foundMototaxista.placa && <p className="text-sm text-pmmg-medium-text">Placa: {foundMototaxista.placa}</p>}
            {foundMototaxista.selo ? (
              <div className="flex items-center text-pmmg-gold mt-2">
                <OfficialSeal size="w-8 h-8" className="mr-2" />
                <span className="text-pmmg-blue font-bold">Participante da Campanha!</span>
              </div>
            ) : (
              <span className="text-pmmg-red font-bold mt-2">Não aderiu à Campanha ainda.</span>
            )}
          </div>
        )}
        {searchMototaxistaTerm && !foundMototaxista && (
            <p className="mt-4 text-pmmg-medium-text text-center">Nenhum resultado encontrado para "{searchMototaxistaTerm}".</p>
        )}
      </div>

      {/* Seção de Suporte Específico */}
      <div className="card-modern">
        <h3 className="text-pmmg-dark-text font-bold text-xl mb-4">Suporte Específico</h3>
        <div className="border border-pmmg-light-border rounded-lg p-4 h-64 flex flex-col overflow-y-auto bg-gray-50">
          <div className="flex-grow space-y-2">
            {supportMessages.length === 0 && <p className="text-center text-pmmg-medium-text">Envie uma mensagem para o suporte.</p>}
            {supportMessages.map((msg, index) => (
              <div key={index} className={`p-2 rounded-lg max-w-[80%] ${msg.sender === 'Estabelecimento' ? 'bg-pmmg-dark-text text-white self-end ml-auto' : 'bg-gray-300 text-pmmg-dark-text self-start mr-auto'}`}>
                <span className="font-bold">{msg.sender}: </span>{msg.text}
              </div>
            ))}
          </div>
          <div className="mt-4 flex">
            <input
              type="text"
              placeholder="Digite sua mensagem para o suporte..."
              value={supportInput}
              onChange={(e) => setSupportInput(e.target.value)}
              className="flex-grow p-2 border-pmmg-light-border rounded-l-lg focus:outline-none focus:ring-2 focus:ring-pmmg-blue"
            />
            <button
              onClick={handleSendSupportMessage}
              className="bg-pmmg-dark-text text-white py-2 px-4 rounded-r-lg hover:bg-gray-800 transition-colors"
            >
              Enviar
            </button>
          </div>
        </div>
      </div>
    </div>
  );
};

// Página de Informações da Campanha (Reutilizada)
const InfoPage = ({ onBack }) => (
  <div className="flex flex-col h-full p-6 bg-pmmg-light-blue-bg rounded-lg shadow-inner overflow-y-auto">
    <h2 className="text-pmmg-blue text-3xl font-bold mb-6 text-center">Informações da Campanha "Avança Poços"</h2>
    <div className="card-modern text-pmmg-dark-text leading-relaxed">
      <p className="mb-4">
        A campanha **"Avança Poços"** é uma iniciativa da Polícia Militar de Minas Gerais em Poços de Caldas, em parceria com a Prefeitura Municipal e diversos órgãos e entidades. Nosso objetivo principal é promover a segurança no trânsito e valorizar os profissionais de mototáxi e motofrete que atuam de forma legalizada na cidade.
      </p>
      <h3 className="text-pmmg-red font-bold text-xl mb-3">Por que esta campanha é importante?</h3>
      <p className="mb-4">
        Com o crescimento dos serviços de entrega e transporte por motocicleta, a informalidade se tornou um desafio. Isso gera riscos para passageiros e para os próprios motociclistas, além de desvalorizar a profissão. Queremos que você saiba como identificar um serviço seguro e como a formalização beneficia a todos.
      </p>
      <h3 className="text-pmmg-red font-bold text-xl mb-3">O Selo Oficial: Sua Garantia de Segurança</h3>
      <p className="mb-4">
        Criamos um **Selo Oficial** que será exibido por todos os mototaxistas e motoentregadores legalizados em Poços de Caldas. Ao escolher um profissional com este selo, você garante que está utilizando um serviço regulamentado, com condutores habilitados e veículos em dia com as normas de segurança.
      </p>
      <div className="flex items-center justify-center bg-pmmg-blue text-white p-4 rounded-lg font-semibold my-6">
        <OfficialSeal size="w-8 h-8" className="mr-3 text-pmmg-gold" />
        <span className="text-xl">Procure sempre pelo Selo Oficial!</span>
      </div>
      <h3 className="text-pmmg-red font-bold text-xl mb-3">Objetivos da Campanha:</h3>
      <ul className="list-disc list-inside mb-4">
        <li>Aumentar a confiança no serviço legalizado.</li>
        <li>Educar a população sobre segurança no trânsito.</li>
        <li>Certificar e valorizar 100% dos mototaxistas legalizados.</li>
        <li>Expandir o número de profissionais formalizados.</li>
        <li>Lançar um aplicativo oficial para sua comodidade e segurança.</li>
      </ul>
      <p className="mb-4">
        **Juntos, Polícia Militar, mototaxistas e cidadãos de Poços de Caldas, construímos um trânsito mais seguro e uma cidade que Avança.**
      </p>
      <p className="text-center text-sm text-pmmg-medium-text mt-6">
        Para mais informações, procure a Polícia Militar de Poços de Caldas ou a Prefeitura Municipal.
      </p>
    </div>
  </div>
);

// Componente Principal do Aplicativo
const App = () => {
  const [userType, setUserType] = useState(null); // null, 'pm', 'mototaxista', 'estabelecimento'
  const [currentPage, setCurrentPage] = useState('home'); // 'home', 'info'

  const getPageTitle = () => {
    if (currentPage === 'info') return 'Informações da Campanha';
    switch (userType) {
      case 'pm': return 'Dashboard PM';
      case 'mototaxista': return 'Dashboard Mototaxista';
      case 'estabelecimento': return 'Dashboard Estabelecimento';
      default: return 'Avança Poços';
    }
  };

  const handleBack = () => {
    if (currentPage === 'info') {
      // If coming from info page, go back to the respective dashboard
      if (userType === 'pm') setCurrentPage('pmDashboard');
      else if (userType === 'mototaxista') setCurrentPage('mototaxistaDashboard');
      else if (userType === 'estabelecimento') setCurrentPage('estabelecimentoDashboard');
    } else {
      setUserType(null); // Go back to login page
      setCurrentPage('home');
    }
  };

  return (
    <div className="min-h-screen flex flex-col bg-gray-50">
      {/* Inserir a configuração do Tailwind CSS dinamicamente */}
      <div dangerouslySetInnerHTML={{ __html: tailwindConfig }} />

      <Header title={getPageTitle()} onBack={userType !== null || currentPage === 'info' ? handleBack : null} />

      <main className="flex-grow flex items-center justify-center p-4">
        <div className="w-full max-w-4xl h-full min-h-[600px] bg-white rounded-xl shadow-2xl overflow-hidden flex flex-col">
          {userType === null ? (
            <LoginPage setUserType={setUserType} />
          ) : currentPage === 'info' ? (
            <InfoPage onBack={handleBack} />
          ) : userType === 'pm' ? (
            <PmDashboard setCurrentPage={setCurrentPage} />
          ) : userType === 'mototaxista' ? (
            <MototaxistaDashboard setCurrentPage={setCurrentPage} />
          ) : userType === 'estabelecimento' ? (
            <EstabelecimentoDashboard setCurrentPage={setCurrentPage} />
          ) : null}
        </div>
      </main>
    </div>
  );
};

export default App;
